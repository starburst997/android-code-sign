# android-publish

Sample project to publish an Android App to the Play Store using Github Action

<br/>

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

Notice that we're using my fork of [joshdholtz/fastlane-plugin-github_action](https://github.com/starburst997/fastlane-plugin-github_action), this is because the published gem is out of date.

Now run:

```console
bundle install
```

<br/>

## Create a Google Play Console Account

[Sign up](https://play.google.com/console/signup) for a **Google Play Console** account, you'll need to pay a one-time fee of $25 and verify your identity. 

You can follow the [instructions here](https://support.google.com/googleplay/android-developer/answer/6112435) but it's pretty straight-forward.

<br/>

## Create a Google Play Service Account

<table align="center"><tr><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-01.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-01.png" alt="Create Google Project" title="Create Google Project"/></a><p align="center">1</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-02.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-02.png" alt="Enable Google Play Developer API" title="Enable Google Play Developer API"/></a><p align="center">2</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-03.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-03.png" alt="Select your project" title="Select your project" /></a><p align="center">3</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-04.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-04.png" alt="Create a new Service Account" title="Create a new Service Account" /></a><p align="center">4</p>
</td></tr></table>

To programmatically access the [Google Play Console](https://play.google.com/console/?hl=en), you will need a dedicated **Google Play Service** account with API access.

The [fastlane documentation](https://docs.fastlane.tools/actions/supply/) highlight the steps but here's a summary:

1. Create a [Google Cloud Project](https://console.cloud.google.com/projectcreate) (or use an existing one). Give it a name and click create ([more info](https://cloud.google.com/resource-manager/docs/creating-managing-projects)).

2. Enable the [Google Play Developer API](https://console.developers.google.com/apis/api/androidpublisher.googleapis.com/?hl=en) by clicking the **ENABLE API** button for your newly created project.

3. Open [Service Accounts](https://console.cloud.google.com/iam-admin/serviceaccounts?hl=en) on Google Cloud and select your project.

4. Create a new **Service Account** by clicking the create button.

<table align="center"><tr><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-05.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-05.png" alt="Fill form, copy email" title="Fill form, copy email"/></a><p align="center">5</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-06.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-06.png" alt="Select Managed Key" title="Select Managed Key"/></a><p align="center">6</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-07.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-07.png" alt="Create new key" title="Create new key" /></a><p align="center">7</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-08.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-08.png" alt="Save JSON key" title="Save JSON key" /></a><p align="center">8</p>
</td></tr></table>

5. Give it a name and ID, copy the resulting **Email address** (we will need it later). Click on **Done** *NOT* "Create and Continue".

6. Click on the vertical **three-dot icon** of your newly created account, and select **Manage keys**.

7. Click on **Add key** and select **Create new key**.

8. Make sure to select **JSON**, click **Create** and save the file on your computer.