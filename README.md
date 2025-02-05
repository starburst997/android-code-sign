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

## Create a Google Service Account

To programmatically access the [Google Play Console](https://play.google.com/console/?hl=en), you will need a dedicated **Google Service** account with API access.

The [fastlane documentation](https://docs.fastlane.tools/actions/supply/) highlight the steps but here's a summary:

<table align="center"><tr><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-01.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-01.png" alt="Create Google Project" title="Create Google Project"/></a><p align="center">1</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-02.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-02.png" alt="Enable Google Play Developer API" title="Enable Google Play Developer API"/></a><p align="center">2</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-03.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-03.png" alt="Select your project" title="Select your project" /></a><p align="center">3</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/google-service-04.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/google-service-04.png" alt="Create a new Service Account" title="Create a new Service Account" /></a><p align="center">4</p>
</td></tr></table>

1. Create a [Google Cloud Project](https://console.cloud.google.com/projectcreate) (or use an existing one). Give it a name and click create ([more info](https://cloud.google.com/resource-manager/docs/creating-managing-projects)).

2. Enable the [Google Play Developer API](https://console.developers.google.com/apis/api/androidpublisher.googleapis.com/?hl=en) by clicking the **Enable API** button for your newly created project.

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

5. Give it a name and ID, copy the resulting **Email address** (we will need it later). Click on **Done**, *NOT* ~Create and Continue~.

6. Click on the vertical **three-dot icon** (under **Actions**) of your newly created account, and select **Manage keys**.

7. Click on **Add key** and select **Create new key**.

8. Make sure to select **JSON**, click **Create** and save the file on your computer.

<br/>

## Invite Service Account to Google Play Console

<table align="center"><tr><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/invite-service-01.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/invite-service-01.png" alt="Invite new user" title="Invite new user"/></a><p align="center">1</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/invite-service-02.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/invite-service-02.png" alt="Paste email address" title="Paste email address"/></a><p align="center">2</p>
</td><td>
<a href="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/invite-service-03.png" target="_blank"><img src="https://jd.boiv.in/assets/posts/2025-02-05-android-publish/small/invite-service-03.png" alt="Choose permissions" title="Choose permissions" /></a><p align="center">3</p>
</td></tr></table>

1. Open the [Google Play Console](https://play.google.com/console/?hl=en) and select **Users and Permissions**. Click **Invite new users**.

2. Paste the **email address** of the **Service Account** you saved for later (`xxxx@yyyy.iam.gserviceaccount.com`).

3. Choose the permissions: **Admin (all permissions)**. Click on **Invite User**.

<br/>

## Test your JSON file

We can now test that the connection is valid using your **JSON file** by running this command:

```sh
bundle exec fastlane run validate_play_store_json_key json_key:/path/to/your/downloaded/file.json
```

Look for `Successfully established connection to Google Play Store`.

<br/>

## Generate an upload key and keystore

You can generate your key using [Android Studio](https://developer.android.com/studio/publish/app-signing#generate-key), but here's how you can do it on the command line using [keytool](https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html) (available in your JDK's bin folder):

```sh
keytool -genkey -v -keystore .keystore -alias android -keyalg RSA -keysize 2048 -validity 10000
```

You'll be prompt to add some information, keep notes of the **password** (use the same for both) and the **alias**.

For ease of use afterward, we'll save the keystore as **base64** and save it as a secret in our repository.

```sh
base64 .keystore
```

I've also created a Github Action that you can run in this repository called: **Generate .keystore**. It will also generate the base64 version.

<br/>

## Save secrets

Blablabla