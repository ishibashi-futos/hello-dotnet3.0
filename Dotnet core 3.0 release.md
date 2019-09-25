# .NET Core 3.0が正式リリースされたので、インストールしてHello Worldしてみた

## .NET Core 3.0のダウンロード

インストールするためにパッケージをダウンロードします。  
ZIPインストールにはまっているので、`.NET Core Binaries:` からダウンロードします。

## 展開、パスの設定

展開、パスを設定後、 `dotnet --version`をしたところ、

```
api-ms-win-crt-filesystem-l1-1-0.dll がないため、プログラムを開始できません
```

などというメッセージが出た。  **Windows のユニバーサル C ランタイム (CRT)** が不足している
というエラーのようで、以下からダウンロードできた。

https://support.microsoft.com/ja-jp/help/2999226/update-for-universal-c-runtime-in-windows

Windows Updateの関係ですでにインストール済みである場合はこのエラーが出ないようだ。

インストール後、再度コマンドを実行すると

```
>dotnet --version
3.0.100
```

できましたね！

## Hello, World(CLI)の実装


何はともあれプロジェクトの作成

```
>dotnet new console

.NET Core 3.0 へようこそ!
---------------------
SDK バージョン: 3.0.100

テレメトリ
---------
.NET Core ツールは、エクスペリエンスの向上のために利用状況データを収集します。データは匿名です。データは Microsoft によって収集され、コミュニティと共有されます。テレメトリをオプトアウトするには、好みのシェルを使用して、DOTNET_CLI_TELEMETRY_OPTOUT 環境変数を '1' または 'true' に設定できます。

.NET Core CLI ツールのテレメトリの詳細をご覧ください: https://aka.ms/dotnet-cli-telemetry

----------------
ドキュメントを確認する: https://aka.ms/dotnet-docs
問題を報告し、GitHub でソースを参照する: https://github.com/dotnet/core
新機能を参照する: https://aka.ms/dotnet-whats-new
インストール済みの HTTPS 開発者の証明書の詳細情報: https://aka.ms/aspnet-core-https
'dotnet --help' を使用して利用可能なコマンドを確認するか、次にアクセスする: https://aka.ms/dotnet-cli-docs
初めてのアプリを作成する: https://aka.ms/first-net-core-app
--------------------------------------------------------------------------------------
Getting ready...
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on <パス>\hello-dotnet3.0\hello-dotnet3.0.csproj...
  <パス>\hello-dotnet3.0\hello-dotnet3.0.csproj の復元が 71.2 ms で完了しました。

Restore succeeded.

```

あとは、runするだけ

```
>dotnet run
Hello World!
```

## ML.NETが動くか試してみた

あまりにも簡単で死にそうだったので、ML.NETも追加してみました。

```
dotnet add package Microsoft.ML
  Writing C:\Users\ishibashi.futos\AppData\Local\Temp\tmpA8E.tmp
info : パッケージ 'Microsoft.ML' の PackageReference をプロジェクト '<PATH>\hello-dotnet3.0\hello-dotnet3.0.csproj' に追加しています。
info : <PATH>\hello-dotnet3.0\hello-dotnet3.0.csproj のパッケージを復元しています...
error: ソース https://api.nuget.org/v3/index.json のサービス インデックスを読み込めません。
error:   Response status code does not indicate success: 407 (Proxy Authentication Required).
```

会社のNWで実行していたのですが、http_proxyを設定しているはずなのにうまく通りませんでした。  
理由はPROXYのユーザ名に@が使われているため、魔法のURLエンコード（%40）をしていたためです。(dotnetコマンドでは@で書くようです)

%40を@に変更して再実行したところ、無事うまくいきました。

```csproj
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
    <RootNamespace>hello_dotnet3._0</RootNamespace>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.ML" Version="1.3.1" />
  </ItemGroup>

</Project>
```

[ML.NETのチュートリアルが親切になっていたのでやってみた。](https://qiita.com/happy_packet/items/f1814524ce8e5bf6883e)の記事を参考（丸パクリ）に、実装しそのまま実行してみましょう！

