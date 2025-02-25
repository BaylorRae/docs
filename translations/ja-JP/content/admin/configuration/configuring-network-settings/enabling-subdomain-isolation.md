---
title: Subdomain Isolationの有効化
intro: 'Subdomain Isolation をセットアップすれば、ユーザーが提供したコンテンツを {% data variables.product.prodname_ghe_server %} アプライアンスの他の部分から安全に分離できるようになります。'
redirect_from:
  - /enterprise/admin/guides/installation/about-subdomain-isolation
  - /enterprise/admin/installation/enabling-subdomain-isolation
  - /enterprise/admin/configuration/enabling-subdomain-isolation
  - /admin/configuration/enabling-subdomain-isolation
versions:
  ghes: '*'
type: how_to
topics:
  - Enterprise
  - Fundamentals
  - Infrastructure
  - Networking
  - Security
shortTitle: Enable subdomain isolation
---

## Subdomain Isolationについて

Subdomain Isolationは、クロスサイトスクリプティングや関連するその他の脆弱性を緩和します。 詳しい情報については"Wikipediaの[クロスサイトスクリプティング](http://en.wikipedia.org/wiki/Cross-site_scripting)"を参照してください。 {% data variables.product.product_location %}ではSubdomain Isolationを有効化することを強くお勧めします。

Subdomain Isolation が有効な場合、{% data variables.product.prodname_ghe_server %} はいくつかのパスをサブドメインで置き換えます。 After enabling subdomain isolation, attempts to access the previous paths for some user-supplied content, such as `http(s)://HOSTNAME/raw/`, may return `404` errors.

| Subdomain Isolationなしのパス               | Subdomain Isolationされたパス                                       |
| -------------------------------------- | -------------------------------------------------------------- |
| `http(s)://HOSTNAME/assets/`           | `http(s)://assets.HOSTNAME/`                                   |
| `http(s)://HOSTNAME/avatars/`          | `http(s)://avatars.HOSTNAME/`                                  |
| `http(s)://HOSTNAME/codeload/`         | `http(s)://codeload.HOSTNAME/`                                 |
| `http(s)://HOSTNAME/gist/`             | `http(s)://gist.HOSTNAME/`                                     |
| `http(s)://HOSTNAME/media/`            | `http(s)://media.HOSTNAME/`                                    |
| `http(s)://HOSTNAME/pages/`            | `http(s)://pages.HOSTNAME/`                                    |
| `http(s)://HOSTNAME/raw/`              | `http(s)://raw.HOSTNAME/`                                      |
| `http(s)://HOSTNAME/render/`           | `http(s)://render.HOSTNAME/`                                   |
| `http(s)://HOSTNAME/reply/`            | `http(s)://reply.HOSTNAME/`                                    |
| `http(s)://HOSTNAME/uploads/`          | `http(s)://uploads.HOSTNAME/`                                  |{% ifversion ghes %}
| `https://HOSTNAME/`                    | `http(s)://docker.HOSTNAME/`{% endif %}{% ifversion ghes %}
| `https://HOSTNAME/_registry/npm/`      | `https://npm.HOSTNAME/`                                        |
| `https://HOSTNAME/_registry/rubygems/` | `https://rubygems.HOSTNAME/`                                   |
| `https://HOSTNAME/_registry/maven/`    | `https://maven.HOSTNAME/`                                      |
| `https://HOSTNAME/_registry/nuget/`    | `https://nuget.HOSTNAME/`{% endif %}{% ifversion ghes > 3.4 %}
| Not supported                          | `https://containers.HOSTNAME/` 
{% endif %}

## 必要な環境

{% data reusables.enterprise_installation.disable-github-pages-warning %}

Subdomain Isolationを有効化する前に、新しいドメインに合わせてネットワークを設定しなければなりません。

- 有効なドメイン名を、IP アドレスではなくホスト名として指定します。 詳しい情報については、「[ホスト名を設定する](/enterprise/{{ currentVersion }}/admin/guides/installation/configuring-a-hostname)」を参照してください。

{% data reusables.enterprise_installation.changing-hostname-not-supported %}

- 上記のサブドメインに対して、ワイルドカードのドメインネームシステム (DNS) レコードまたは個々の DNS レコードをセットアップします。 各サブドメイン用に複数のレコードを作成せずに済むよう、サーバのIPアドレスを指す`*.HOSTNAME`のAレコードを作成することをおすすめします。
- `HOSTNAME` とワイルドカードのドメイン `*.HOSTNAME` の両方に対するサブジェクト代替名 (SAN) が記載された、`*.HOSTNAME` に対するワイルドカードの Transport Layer Security (TLS) 証明書を取得します。 たとえば、ホスト名が `github.octoinc.com` である場合は、Common Name の値が `*.github.octoinc.com` に設定され、SAN の値が `github.octoinc.com` と `*.github.octoinc.com` の両方に設定された証明書を取得します。
- アプライアンスで TLS を有効にします。 詳しい情報については"[TLSの設定](/enterprise/{{ currentVersion }}/admin/guides/installation/configuring-tls/)"を参照してください

## Subdomain Isolationの有効化

{% data reusables.enterprise_site_admin_settings.access-settings %}
{% data reusables.enterprise_site_admin_settings.management-console %}
{% data reusables.enterprise_management_console.hostname-menu-item %}
4. **Subdomain isolation (recommended)（Subdomain Isolation（推奨））**を選択してください。 ![Subdomain Isolation を有効化するチェックボックス](/assets/images/enterprise/management-console/subdomain-isolation.png)
{% data reusables.enterprise_management_console.save-settings %}
