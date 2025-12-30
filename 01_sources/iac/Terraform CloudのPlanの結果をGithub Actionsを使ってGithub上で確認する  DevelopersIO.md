---
title: "Terraform CloudのPlanの結果をGithub Actionsを使ってGithub上で確認する | DevelopersIO"
source: "https://dev.classmethod.jp/articles/terraform-cloud-plan-result-githubactions/"
author:
  - "[[佐藤雅樹]]"
published: 2023-01-20
created: 2025-11-18
description:
tags:
  - blog
  - cicd
  - github
  - iac
  - japanese
  - terraform
  - tutorial
---
**「Terraform Cloudのplanの結果を、Github上で見たいな」**

Terraform Cloud上で以下のように terraform planの結果を確認することができます。

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/run-HPWTWpGMagHR7Aa1___Runs___learn-sentinel-tfc-sato___classmethod-sandbox___Terraform_Cloud-960x1177.png)

表示も見やすくて、この機能はとても便利です。

ただ、TerraformコードへのPull Request時にコードと一緒にPlanを確認するといった運用をする場合を考えてみます。

その際に、GithubとTerraform Cloudの画面を行き来するのが面倒だと感じる人もいると思います。

「Github上で確認するいい方法がないかな」と思っていたら、以下のチュートリアルを見つけました。

[Automate Terraform with GitHub Actions | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions#create-pull-request)

今回は、Terraform Cloudで実行されるPlanの結果をGithubActionsを使って、GithubのPull Request上で確認してみます。

最終的には、以下のように確認できます。

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/05f2e680ae145383af8dcda933b0efff-960x1291.png)

## 構成図

今回の構成をざっくり図にしました。

Terraform Cloudにて `terraform plan` を実行するため、Github ActionsにはAWS環境の認証情報は不要です。

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/Terraform-Cloud-GithubActions.png)

## やってみた

チュートリアル上では、新規にGithubリポジトリとTerraform Cloudのワークスペースを作成しています。

少し変えて、今回は既存のGithubリポジトリとワークスペースに対して設定してみます。

既存に変更する関係で、Workspaceに対するAWSの認証情報の設定は完了している状態で作業しています。

WorkspaceにAWS認証情報を設定していない場合は、何らかの方法でAWS認証情報を設定してください。

[Create a Credentials Variable Set | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-create-variable-set)

### WorkspaceをAPI-driven Workflowに変更

Github Actions上でTerraform CloudのAPIを叩く必要があるため、API-drivenにWorkflowを変更します。

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/ddedb6cd436c1f78f3947f4a90ad39c8-960x598.png)

### Terraform Cloud API Tokenを発行・GithubリポジトリのSecretsに登録

[Token Page](https://app.terraform.io/app/settings/tokens?utm_source=learn) に遷移して、Tokenを発行します。 Github Actionsで使用するので、 `description` は `Github Actions` としました。

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/Tokens___Account___Terraform_Cloud-960x434.png)

createを押すとTokenが表示されるので、コピーしておきます。

その後、Githubの設定したいリポジトリに移動して、 `Secrets` 　に登録します。

- Name: `TF_API_TOKEN`
- Secrets: `[Terraform Cloudで作成したToken]`

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/c9b95d05278410613f05f9a6d33d0dc2-960x724.png)

### GithubActionsのワークフローファイルを用意

```
$ mkdir -p .github/workflows
$ vi .github/workflows/terraform.yml
```

```
name: 'Terraform'

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  terraform:
    name: 'Terraform'
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          cli_config_credentials_token: ${{ secrets.TF_API_TOKEN }}

      - name: Terraform Format
        id: fmt
        run: terraform fmt -check

      - name: Terraform Init
        id: init
        run: terraform init

      - name: Terraform Validate
        id: validate
        run: terraform validate -no-color

      - name: Terraform Plan
        id: plan
        if: github.event_name == 'pull_request'
        run: terraform plan -no-color -input=false
        continue-on-error: true

      - name: Update Pull Request
        uses: actions/github-script@v6
        if: github.event_name == 'pull_request'
        env:
          PLAN: "terraform\n${{ steps.plan.outputs.stdout }}"
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const output = \`#### Terraform Format and Style ?\\`${{ steps.fmt.outcome }}\\`
            #### Terraform Initialization ⚙️\\`${{ steps.init.outcome }}\\`
            #### Terraform Plan ?\\`${{ steps.plan.outcome }}\\`
            #### Terraform Validation ?\\`${{ steps.validate.outcome }}\\`

            <details><summary>Show Plan</summary>

            \\`\\`\\`\n
            ${process.env.PLAN}
            \\`\\`\\`

            </details>

            *Pushed by: @${{ github.actor }}, Action: \\`${{ github.event_name }}\\`*\`;

            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: output
            })

      - name: Terraform Plan Status
        if: steps.plan.outcome == 'failure'
        run: exit 1

      - name: Terraform Apply
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve -input=false
```

### Pull Requestの作成

Terraformコード上に任意の変更を行なって、Pull Requestを作成します。

以下のようにPull Request上でPlanの結果が確認できました。

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/05f2e680ae145383af8dcda933b0efff-1-960x1291.png)

Terraform Cloud上でコマンドが実行されるため、Terraform Cloudのコンソールからも実行結果は確認できます。

![](https://devio2023-media.developers.io/wp-content/uploads/2023/01/run-mhQEmgrQVPmBvKdg___Runs___sato-sandbox___classmethod-sandbox___Terraform_Cloud-960x859.png)

## おわりに

Terraform Cloudの実行結果をGithubのPull Request上で確認する方法でした。

レビュー時にTerraform CloudとGithubを行き来しなくて良くなりますね。

Github Actions分の実行時間を消費するというデメリットはありますが、この仕組みを使えばTerraform CloudにログインできないユーザーもPlanの結果確認やApplyの実行(今回はPRのマージをトリガーに設定)ができます。

以上、AWS事業本部の佐藤([@chari7311](https://twitter.com/chari7311))でした。

この記事をシェアする