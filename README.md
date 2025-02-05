# android-publish

Sample project to publish an Android App to the Play Store using Github Action

## Create Github Repository

Create a Github Repository for your project. Feel free to use [this repository](https://github.com/starburst997/android-publish) as a template since it already contains all the workflows files.

<br/>

## Installing fastlane

We need to install [fastlane](https://fastlane.tools/) locally on our machine, first makes sure you have [ruby](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller) installed (personally I prefer via [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install)).

Also you need to makes sure [Bundler](https://bundler.io/) is installed:

```console
gem install bundler
```

Now create a file called `Gemfile` in the root:

```ruby
source "https://rubygems.org"
gem "fastlane"
gem 'fastlane-plugin-github_action', git: "https://github.com/starburst997/fastlane-plugin-github_action"
```

Notice that we're using my fork of [joshdholtz/fastlane-plugin-github_action](https://github.com/starburst997/fastlane-plugin-github_action), this is because the published gem is out of date and also because there was an issue preventing us to re-use the same repository for `fastlane match` again for other projects.

Now run:

```console
bundle install
```

<br/>

