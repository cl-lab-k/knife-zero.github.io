---
layout: tips-ja
lang: ja
title: Cookbookマネージャと一緒に
permalink: tips/with_cookbook_manager-ja/
---

この項目内では3rdパーティーのCookbookをマネージャで管理して、自家製のCookbookを`CHEF_REPO/site-cookbooks`に設置する前提にしています。

自家製CookbookもCookbookマネージャで管理している場合は、`CHEF_REPO/site-cookbooks`は不要です。

## Librarian

参考: [librarian-chef#knife-integration](https://github.com/applicationsonline/librarian-chef#knife-integration)

Gemfileに`librarian-chef`を追加してbundleします。

```
gem 'knife-zero'
gem 'librarian-chef'
```

`knife.rb`で`cookbook_path`を次のようにして下さい。

```ruby
require 'librarian/chef/integration/knife'
cookbook_path Librarian::Chef.install_path,
              "/path/to/chef-repo/site-cookbooks"
```

例えば、Chef−Repoのルートに`knife.rb`を置いている環境なら次のように記述します。

```ruby
require 'librarian/chef/integration/knife'
cookbook_path Librarian::Chef.install_path,
              File.expand_path("../site-cookbooks", __FILE__)
```

設定後、knife実行時に`Cheffile and Cheffile.lock are out of sync!`が出る場合、次のどちらかで解決しましょう。

- `librarian-chef install`
- `librarian-chef clean`

## Berkshelf

`berks vendor`コマンドで`Berksfile`に記述されたCookbookを`cookbooks/`に収容します。

```
$ berks vendor cookbooks
```

`knife.rb`で`cookbook_path`を次のようにして下さい。

```ruby
cookbook_path File.expand_path("../cookbooks", __FILE__),
              "/path/to/chef-repo/site-cookbooks")
```

例えば、Chef−Repoのルートに`knife.rb`を置いている環境なら次のように記述します。

```ruby
cookbook_path File.expand_path("../cookbooks", __FILE__),
              File.expand_path("../site-cookbooks", __FILE__)
```

ただこの方法ではBerkshelfとの結合度はゆるいため、矛盾には注意しましょう。

`berks verify`を組み込むといいかもしれまん。


## Policyfile

WIP
