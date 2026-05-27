# AWS Amplify Gen 2 (JavaScript): Get started › Quickstart

## 環境
- Windows 11 Pro, 25H2, 26200.8457
- PowerShell 7.6.1 (LTS)
- Windows Subsystem for Linux (WSL2), Ubuntu 24.04.4 (LTS), Linux 6.6.114.1-microsoft-standard-WSL2
- AWS CLI version 2.34.51 Python/3.14.4
- Node.js 24.15.0 (LTS), npm 11.12.1

## 全体図
```
/
    +-home
    |     +-mizuki
    |           +-download
    |           |     |-awscliv2.zip
    |           |
    |           +-workspace
    |                 +-aws-amplify-gen2
    |                       +-quickstart
    |                             +-amplify-js-app
    |
    +-usr
          +-local
                +-bin
                      |-aws -> /usr/local/aws-cli/v2/current/bin/aws
```

## 1. WSLの環境を最新に更新する
WSLバージョンを最新バージョンに更新する。PowerShell上で以下のコマンドを実行する。UACのダイアログが出る。実行中のUbuntuのタブは先に閉じておく。
```
wsl --update
```

実行結果。
```
更新プログラムを確認しています。
Linux 用 Windows サブシステムをバージョン 2.7.3 に更新しています。
```

WSLとそのコンポーネントに関するバージョン情報を確認する。PowerShell上で以下のコマンドを実行する。
```
wsl --version
```

実行結果。
```
WSL バージョン: 2.7.3.0
カーネル バージョン: 6.6.114.1-1
WSLg バージョン: 1.0.73
MSRDC バージョン: 1.2.6676
Direct3D バージョン: 1.611.1-81528511
DXCore バージョン: 10.0.26100.1-240331-1435.ge-release
Windows バージョン: 10.0.26200.8457
```

ディストリビューションのパッケージの更新とアップグレードを実施する。Ubuntu上で以下のコマンドを実行する。
```
sudo apt update && sudo apt upgrade
```

現在の環境を確認する。
```
lsb_release -a
```

実行結果。
```
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble
```

現在の環境を確認する。
```
cat /etc/os-release
```

実行結果。
```
PRETTY_NAME="Ubuntu 24.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.4 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

現在の環境を確認する。
```
uname -a
```

実行結果。
```
Linux SilentMasterPro 6.6.114.1-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Mon Dec  1 20:46:23 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
```

参考情報。
- [Update WSL | Basic commands for WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#update-wsl)
- [Check WSL version | Basic commands for WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#check-wsl-version)
- [Update and upgrade packages | Set up a WSL development environment | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/setup/environment#update-and-upgrade-packages)

## 2. AWS上にユーザーを作成する
このユーザーをアプリケーションの開発と利用に使用する。  

ユーザーを追加する。メールアドレスは全ユーザーで一意である必要がある。
1. IAM Identity Centerコンソールを開く。
2. 左側メニューの「ユーザー」をクリックする。
3. 右側の「ユーザーを追加」ボタンをクリックする。
4. ユーザーの詳細を指定して「次へ」ボタンをクリックする。プライマリ情報の「ユーザー名」「メールアドレス」「名」「姓」のみ指定する。
5. 「ユーザーをグループに追加」をスキップして「次へ」ボタンをクリックする。
6. 「ユーザーの確認と追加」画面で内容を確認し、「ユーザーを追加」ボタンをクリックする。
7. ユーザーに招待メールが届く。メール内の「Accept Invitation」ボタンをクリックして招待を受け入れる。「新規ユーザーのサインアップ」画面をメールのアプリ内ブラウザではなく明示的に外部ブラウザ(Chrome)で開く。安全なパスワードを自動生成してパスワードマネージャーにユーザー名といっしょに保存して「新しいパスワードを設定」ボタンをクリックする。サインイン画面でユーザー名とパスワードを入力してAWSアクセスポータルの画面が表示されたら成功となる。

グループを追加する。
1. IAM Identity Centerコンソールを開く。
2. 左側メニューの「グループ」をクリックする。
3. 右側の「グループを作成」ボタンをクリックする。
4. グループの詳細を指定して「グループを作成」ボタンをクリックする。「グループ名」に「amplify」のみ指定する。オプションの「ユーザーをグループに追加」でユーザーを選択する。

権限セットを作成する。
1. IAM Identity Centerコンソールを開く。
2. 左側メニューの「マルチアカウント許可」を開いて「許可セット」をクリックする。
3. 右側の「許可セットを作成」ボタンをクリックする。
4. 「許可セットタイプを選択」画面で許可セットのタイプを「カスタム許可セット」を選択して「次へ」ボタンをクリックする。
5. 「ポリシーと許可の境界を指定」画面で「AWSマネージドポリシー」を開いて「AmplifyBackendDeployFullAccess」を選択して「次へ」ボタンをクリックする。
6. 「許可セットの詳細を指定」画面で許可セットの詳細を指定して「次へ」ボタンをクリックする。許可セット名「amplify-backend」のみ指定する。
7. 「確認して作成」画面で内容を確認し、「作成」ボタンをクリックする。

AWSアカウントへのアクセス権を設定する。
1. IAM Identity Centerコンソールを開く。
2. 左側メニューの「マルチアカウント許可」を開いて「AWSアカウント」をクリックする。
3. 右側でアクセス権を設定するアカウントを選択し、「ユーザーまたはグループを割り当て」ボタンをクリックする。
4. 「ユーザーとグループの選択」画面で「グループ」タブを表示し、作成した「amplify」グループを選択する。「次へ」ボタンをクリックする。
5. 「許可セットを選択」画面で作成した「amplify-backend」許可セットを選択し、「次へ」ボタンをクリックする。
6. 「確認して送信」画面で内容を確認し、「送信」ボタンをクリックする。

ここまでで、ユーザー < グループ(権限セット) < AWSアカウントの関係が作成される。  

参考情報。
- [Users, groups, and provisioning in IAM Identity Center | User Guide | AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/users-groups-provisioning.html)
- [Add users to your Identity Center directory | User Guide | AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/addusers.html)
- [Add groups to your Identity Center directory | User Guide | AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/addgroups.html)
- [Create a permission set | User Guide | AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocreatepermissionset.html)
- [Assign user or group access to AWS accounts | User Guide | AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/assignusers.html)

## 3-A. Ubuntu上にAWS CLIをインストールする
unzipコマンドがインストールされていないことを確認する。
```
unzip -v
```

実行結果。コマンドが見つからないエラーになる。
```
Command 'unzip' not found, but can be installed with:
sudo apt install unzip
```

unzipコマンドをインストールする。
```
sudo apt install unzip
```

インストールしたunzipコマンドのバージョンを確認する。
```
unzip -v
```

実行結果。
```
UnZip 6.00 of 20 April 2009, by Debian. Original by Info-ZIP.
```

AWS CLIコマンドがインストールされていないことを確認する。
```
aws --version
```

実行結果。コマンドが見つからないエラーになる。
```
Command 'aws' not found, but can be installed with:
sudo snap install aws-cli  # version 1.44.61, or
sudo apt  install awscli   # version 2.14.6-1
See 'snap info aws-cli' for additional versions.
```

AWS CLI version 2をインストールする。lsコマンドで以前にダウンロードしたファイルが存在するか確認する。存在している場合は削除してから作業を進める。
```
cd /home/mizuki/download
ls -al
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

実行結果。インストールは一瞬で終わるかもしれない。
```
You can now run: /usr/local/bin/aws --version
```

インストールしたAWS CLIコマンドのバージョンを確認する。
```
aws --version
```

実行結果。
```
aws-cli/2.34.26 Python/3.14.3 Linux/6.6.87.2-microsoft-standard-WSL2 exe/x86_64.ubuntu.24
```

参考情報。
- [Installing or updating to the latest version of the AWS CLI | User Guide for Version 2 | AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

## 3-B. Ubuntu上のAWS CLIを更新する
downloadディレクトリにあるAWS CLIの既存のファイルを削除する。
```
cd /home/mizuki/download
rm awscliv2.zip
rm -rf aws
```

--bin-dirオプションを使用してシンボリックリンクが存在するディレクトリを指定するのでwhichコマンドで確認する。
```
which aws
```

実行結果。
```
/usr/local/bin/aws
```

--install-dirオプションを使用してシンボリックリンクが指すインストール先のディレクトリを指定するのでlsコマンドで確認する。
```
ls -l /usr/local/bin/aws
```

実行結果。
```
lrwxrwxrwx 1 root root 37 Apr  8 11:41 /usr/local/bin/aws -> /usr/local/aws-cli/v2/current/bin/aws
```

AWS CLIをダウンロードして更新する。
```
cd /home/mizuki/download
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```

更新したAWS CLIのバージョンを確認する。
```
aws --version
```

実行結果。
```
aws-cli/2.34.51 Python/3.14.4 Linux/6.6.114.1-microsoft-standard-WSL2 exe/x86_64.ubuntu.24
```

参考情報。
- [Installing or updating to the latest version of the AWS CLI | User Guide for Version 2 | AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [CHANGELOG | CLI Releases | Universal Command Line Interface for Amazon Web Services (v2)](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst)

## 4-A. AWS CLIを設定する
ローカル開発環境のAWSのプロファイルを作成する。
```
cd /home/mizuki
aws configure sso
```

実行結果。プロンプトに従って入力する。
```
SSO session name (Recommended): <AWS上に作成したユーザー名>
SSO start URL [None]: <AWSアクセスポータルのURL(IAM Identity Center > 左側メニューの「設定」 > 右側の「アイデンティティソースタブ」 > AWS access portal URL > IPv4専用)>
SSO region [None]: <AWSアカウントのリージョン(IAM Identity Center > 左側メニューの「設定」 > 右側の「詳細」のプライマリリージョン？)>
SSO registration scopes [sso:account:access]: <デフォルトの空白文字を残したままEnterキーを押す>
```

実行結果。WSL上のUbuntuからはブラウザが開かない。表示されたURLをコピーして、Windows側のブラウザで開く。
```
Attempting to open your default browser. If the browser does not open, open the following URL.
If you are unable to open the URL on this device, run this command again with the '--use-device-code' option.

(URLが表示される)
```

ブラウザで開いたサインイン画面で「ユーザー名」を入力して「次へ」ボタンをクリックする。「パスワード」を入力して「サインイン」ボタンをクリックする。  

実行結果。
```
botocore-client-<ユーザー名>がデータにアクセスすることを許可しますか？
```

「アクセスを許可」ボタンをクリックする。  

実行結果。「Sign in to AWS」画面が表示される。ブラウザを閉じる。
```
Your credentials have been shared successfully and can be used until your session expires. You can now close this tab.
```

ターミナルにプロンプトの続きが表示される。
```
The only AWS account available to you is: <AWSアカウントID>
Using the account ID <AWSアカウントID>
The only role available to you is: <許可セット名>
Using the role name "<許可セット名>"
Default client Region [None]: <AWSアカウントのリージョン(IAM Identity Center > 左側メニューの「設定」 > 右側の「詳細」のプライマリリージョン？)>
Default output format (json if not specified) [None]: <デフォルトの空白文字を残したままEnterキーを押す>
Profile name [<許可セット名-AWSアカウントID>]: default <Enterキーを押す>

The AWS CLI is now configured to use the default profile.
Run the following command to verify your configuration:

aws sts get-caller-identity
```

作成されたプロファイルの内容を確認する。
```
aws sts get-caller-identity
```

実行結果。現在の認証状態がJSON形式で表示される。
```
{
    "UserId": "<<ユーザー名>が含まれる文字列>",
    "Account": "<AWSアカウントID>",
    "Arn": "<<許可セット名>が含まれる文字列>"
}
```

参考情報。
- [4. Set up local AWS profile](https://docs.amplify.aws/javascript/start/account-setup/#4-set-up-local-aws-profile)
- [get-caller-identity | AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html#get-caller-identity)

## 4-B. AWS CLI用のSSOのトークンを取得する
AWS CLI用のSSOのトークンを取得してCLIからAWSリソースにアクセスできるようにする。
```
aws sso login --profile default
```

実行結果。httpsから始まるURLをコピーしてWindows側のブラウザで開く。AWS用に作成したユーザーでサインインする。
```
Attempting to open your default browser. If the browser does not open, open the following URL.
If you are unable to open the URL on this device, run this command again with the '--use-device-code' option.

(1つ目にhttpsから始まるURLが表示される)
(2つ目にgio:httpsから始まるURLが表示される。GNOME上のデフォルトブラウザ起動用らしい)
```

実行結果。ブラウザでサインインが成功すると、ターミナルにプロンプトの続きが表示される。
```
Successfully logged into Start URL: https://xxx.awsapps.com/start
```

## 5. Ubuntu上にNode.jsをインストールする
Node.jsの最新のLTSバージョンをインストールする。インストール済みの場合は最新バージョンに更新する。
```
# nvmをダウンロードしてインストールする：
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# シェルを再起動する代わりに実行する
\. "$HOME/.nvm/nvm.sh"

# インストール可能なNode.jsのバージョンを確認する：
nvm ls-remote

# Node.jsの最新のLTSバージョンをダウンロードしてインストールする：
nvm install --lts

# Node.jsのバージョンを確認する：
node -v # "v24.15.0"が表示される。

# npmのバージョンを確認する：
npm -v # "11.12.1"が表示される。
```

参考情報。
- [nvm - Node Version Manager](https://github.com/nvm-sh/nvm)
- [Node.js - どこでもJavaScriptを使おう](https://nodejs.org/ja/)

## 6. ローカル開発環境を構築する
プロジェクトのディレクトリ構造を作成する。
```
mkdir -p /home/mizuki/workspace/aws-amplify-gen2/quickstart
```

プロジェクトを作成する。
```
cd /home/mizuki/workspace/aws-amplify-gen2/quickstart
npm create vite@latest
```

選択肢を選んで実行する。
```
Need to install the following packages:
create-vite@9.0.5
Ok to proceed? (y) y

> npx
> "create-vite"

Project name:
amplify-js-app

Select a framework:
Vanilla

Select a variant:
TypeScript

Install with npm and start now?
Yes
```

Webサーバーが起動する。Windows側からブラウザでアクセスすると画面が表示される。Ctrl+Cで終了する。
```
VITE v8.0.14 ready in 136ms

Local:    http://localhost:5173/
Network:  use --host to expose
press h + enter to show help
```

Ubuntu上でvscodeを起動してTodoアプリのフロントエンドコードを作成する。
```
cd /home/mizuki/workspace/aws-amplify-gen2/quickstart
code .
```

/home/mizuki/workspace/aws-amplify-gen2/quickstart/amplify-js-app/index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo App</title>
</head>
<body>
    <main>
        <h1>My todos</h1>
        <button id="addTodo">+ new</button>
        <ul id="todoList"></ul>
        <div>
            Try creating a new todo.
            <br>
            <a href="https://docs.amplify.aws/javascript/start/quickstart/">
                Review next step of this tutorial.
            </a>
        </div>
    </main>
    <script type="module" src="src/main.ts"></script>
</body>
</html>
```

/home/mizuki/workspace/aws-amplify-gen2/quickstart/amplify-js-app/src/style.css
```
body {
  margin: 0;
  background: linear-gradient(180deg, rgb(117, 81, 194), rgb(255, 255, 255));
  display: flex;
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-items: center;
}

main {
  display: flex;
  flex-direction: column;
  align-items: stretch;
}

button {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #1a1a1a;
  cursor: pointer;
  transition: border-color 0.25s;
  color: white;
}
button:hover {
  border-color: #646cff;
}
button:focus,
button:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}

ul {
  padding-inline-start: 0;
  margin-block-start: 0;
  margin-block-end: 0;
  list-style-type: none;
  display: flex;
  flex-direction: column;
  margin: 8px 0;
  border: 1px solid black;
  gap: 1px;
  background-color: black;
  border-radius: 8px;
  overflow: auto;
}

li {
  background-color: white;
  padding: 8px;
}

li:hover {
  background: #dadbf9;
}

a {
  font-weight: 800;
  text-decoration: none;
}
```

/home/mizuki/workspace/aws-amplify-gen2/quickstart/amplify-js-app/src/main.ts
```
(既存のコードを削除して空にする)
```

Webサーバーを起動して画面を確認する。
```
cd /home/mizuki/workspace/aws-amplify-gen2/quickstart/amplify-js-app
npm run dev
```

プロジェクトにAmplifyのバックエンドを追加する。
```
cd /home/mizuki/workspace/aws-amplify-gen2/quickstart/amplify-js-app
npm create amplify@latest
```

実行結果。
```
Need to install the following packages:
create-amplify@1.3.0
Ok to proceed? (y) y[Enter]

? Where should we create your project? (.) .[Enter]

4:29:47 PM Installing devDependencies:
4:29:47 PM  - @aws-amplify/backend
4:29:47 PM  - @aws-amplify/backend-cli
4:29:47 PM  - aws-cdk-lib@2.234.1
4:29:47 PM  - constructs@^10.0.0
4:29:47 PM  - typescript@^5.0.0
4:29:47 PM  - tsx
4:29:47 PM  - esbuild

4:29:47 PM Installing dependencies:
4:29:47 PM  - aws-amplify

4:29:47 PM ✔ 4:31:28 PM DevDependencies installed
4:31:28 PM ✔ 4:31:39 PM Dependencies installed
4:31:39 PM ✔ 4:31:39 PM Template files created
4:31:39 PM Successfully created a new project!

4:31:39 PM Welcome to AWS Amplify!
4:31:39 PM  - Get started by running npx ampx sandbox.
4:31:39 PM  - Run npx ampx help for a list of available commands.

4:31:39 PM Amplify collects anonymous telemetry data about general usage of the CLI. Participation is optional, and you may opt-out by using npx ampx configure telemetry disable. To learn more about telemetry, visit https://docs.amplify.aws/react/reference/telemetry
```

Amplifyのバックエンドをクラウドのサンドボックスにデプロイする。
```
cd /home/mizuki/workspace/aws-amplify-gen2/quickstart/amplify-js-app
npx ampx sandbox
```

実行結果。
```
4:47:31 PM [Sandbox] Pattern !.vscode/extensions.json found in .gitignore. ".vscode/extensions.json" will not be watched if other patterns in .gitignore are excluding it.

  Amplify Sandbox

  Identifier:   mizuki
  Stack:        amplify-amplifyjsapp-mizuki-sandbox-xxx
  Region:       <AWSのリージョン名>

  To specify a different sandbox identifier, use --identifier

4:47:37 PM ✔ Backend synthesized in 3.42 seconds
4:47:42 PM ✔ Type checks completed in 5.16 seconds
4:48:01 PM ✔ Built and published assets
4:51:39 PM ✔ Deployment completed in 218.756 seconds
4:51:39 PM AppSync API endpoint = https://xxx.appsync-api.<AWSのリージョン名>.amazonaws.com/graphql
4:51:39 PM [Sandbox] Watching for file changes...
4:51:43 PM File written: amplify_outputs.json
```

Ctrl+Cでサンドボックスを終了する。
```
^C
Stopping the sandbox process. To delete the sandbox, run npx ampx sandbox delete
```

サンドボックスを削除する。
```
npx ampx sandbox delete
```

実行結果。
```
✔ Are you sure you want to delete all the resources in your sandbox environment (This can't be undone)? y
5:34:57 PM [Sandbox] Deleting all the resources in the sandbox environment...
5:36:45 PM ✔ Deployment completed in 103.955 seconds
5:36:45 PM [Sandbox] Finished deleting.
```
