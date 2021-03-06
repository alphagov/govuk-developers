---
owner_slack: "#govuk-developers"
title: Get started on GOV.UK
description: Guide for new developers on GOV.UK
layout: manual_layout
section: Learning GOV.UK
---

This getting started guide is for new technical staff (for example developers, technical architects) working on GOV.UK in [GDS][] or CDDO.

Ask your tech lead to take you through the [overview slides][overview-slides] if they have not already done so.

If you're having trouble with this guide, you can ask your colleagues on the [#govuk-developers Slack channel](https://gds.slack.com/archives/CAB4Q3QBW) or using email.

[GDS]: https://gds.blog.gov.uk/about/
[overview-slides]: https://docs.google.com/presentation/d/1nAE65Og04JYNAc0VjYaUYLqNLuUOM9r3Mvo0PGFy_Zk

## Before you start

You must have a laptop with full admin access. To check if you have full admin access, run a `sudo` command in your command line. For example, `sudo ls`.

If you do not have full admin access to your laptop, ask your line manager to ask IT to provide you with a developer build on your laptop.

Once you have full admin access on your laptop, run the following in your command line to install the Xcode command line tool:

```
xcode-select --install
```

## 1. Install the Homebrew package manager

Run the following in your command line to install the [Homebrew package manager](https://brew.sh):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

This command works for macOS or Linux.

## 2. Set up your GitHub account

1. Set up a [GitHub] account. You can use your existing personal account.
1. Ask your tech lead to add you to the [alphagov organisation][alphagov] and the [GOV.UK team][govuk-team] to get access to repos & CI.

    Select __Accept__ in the GitHub email invitation when you receive this email. If your tech lead is not available, ask one of the [GDS GitHub owners](https://groups.google.com/a/digital.cabinet-office.gov.uk/g/gds-github-owners/members?pli=1).
1. Ask your tech lead to add your GitHub username to the [user monitoring system][user-reviewer].

    If your tech lead is not available, ask in the [2nd line support Slack channel](https://gds.slack.com/archives/CADKZN519) for someone who has production access to add you.
1. [Generate a new SSH key for your laptop and add it to the ssh-agent][generate-ssh-key] for your GitHub account.
1. [Add the SSH key to your GitHub account][add-ssh-key].
1. Add the following code into your `.zshrc`, `~/.bash_profile`, or equivalent so that it is persistent between restarts:

    ```
    $ /usr/bin/ssh-add -K <YOUR-PRIVATE-KEY>
    ```

1. Test that the SSH key works by running `ssh -T git@github.com`.
1. Add your name and email to your git commits. For example:

    ```
    $ git config --global user.email "friendly.giraffe@digital.cabinet-office.gov.uk"
    $ git config --global user.name "Friendly Giraffe"
    ```

[alphagov]: https://github.com/alphagov
[GitHub]: https://www.github.com/
[govuk-team]: https://github.com/orgs/alphagov/teams/gov-uk/members
[register-ssh-key]: https://help.github.com/articles/connecting-to-github-with-ssh/
[generate-ssh-key]: https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
[add-ssh-key]: https://docs.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account
[user-reviewer]: https://github.com/alphagov/govuk-user-reviewer

## 3. Install GDS command line tools

On GOV.UK we use the following command-line tools for AWS and SSH access:

- [`govuk-connect`](https://github.com/alphagov/govuk-connect)
- [`gds-cli`](https://github.com/alphagov/gds-cli)

1. To install these command line tools, run the following in the command line:

    ```bash
    brew tap alphagov/gds
    brew install gds-cli govuk-connect
    ```

    The GDS CLI repository is private, so you must first [set up your GitHub account](#2-set-up-your-github-account).

1. Test that both tools work by running `gds --help` and `gds govuk connect --help`.

    If you see a `fatal: no such path in the working tree` error, that's because you're using ZSH, which has `gds` set up as a Git alias. To solve this, you can either:
      - remove that alias by adding `unalias gds` to your `~/.zshrc`
      - use `gds-cli` instead of `gds` for all the relevant commands

1. Configure the GDS CLI by running:

    ```bash
    gds config email <FIRSTNAME>.<LASTNAME>@digital.cabinet-office.gov.uk
    gds config yubikey <true>/<false>
    ```

1. Run `gds config yubikey false` if you use your phone as an Multi-Factor Authentication (MFA) device.

    Run `gds config yubikey true` if you use a Yubikey.

## 4. Connect to the GDS VPN

If you're outside of the office or on [GovWiFi](https://sites.google.com/a/digital.cabinet-office.gov.uk/gds/we-are-gds/service-design-and-assurance/govwifi), you must connect to the GDS VPN to access to our infrastructure and internal services.

To connect to the GDS VPN, follow the [Cabinet Office guidance on how to sign into the GDS VPN using Google credentials][gds-vpn-wiki].

[gds-it-helpdesk]: https://gdshelpdesk.digital.cabinet-office.gov.uk/helpdesk/WebObjects/Helpdesk.woa
[gds-vpn-wiki]: https://docs.google.com/document/d/1O1LmLByDLlKU4F1-3chwS8qddd2WjYQgMaaEgTfK5To/edit

## 5. Set up GOV.UK Docker

We use a `govuk-docker` Docker environment for local development.

To set up GOV.UK Docker, see the [installation instructions in the `govuk-docker` GitHub repo][govuk-docker].

[govuk-docker]: https://github.com/alphagov/govuk-docker/blob/master/README.md

## 6. Get SSH access to integration

### Get access

Ask your tech lead or [GOV.UK 2nd line Support on Slack](https://gds.slack.com/archives/CADKZN519) to add your SSH username (`firstnamelastname`) to the [list of GOV.UK tech users](https://github.com/alphagov/govuk-user-reviewer/blob/master/config/govuk_tech.yml) in the [user monitoring system][user-reviewer].

[user-reviewer]: https://github.com/alphagov/govuk-user-reviewer

### Create a user to SSH into integration

User accounts in our integration environments are managed in the [govuk-puppet][] repository.

1. Run the following command to create a `govuk` folder in your home directory and clone the `govuk-puppet` GitHub repo:

    ```bash
    mkdir ~/govuk
    cd ~/govuk
    git clone git@github.com:alphagov/govuk-puppet.git
    ```

1. Run `more ~/.ssh/alphagov.pub` and copy your outputted SSH public key value.

    The key should begin with `ssh-rsa AAA` and end with `== <GITHUB EMAIL>`. You may need to manually add the email address to the end of your key.

1. Create a user manifest file at `~/govuk/govuk-puppet/modules/users/manifests/<FIRSTNAMELASTNAME>.pp` with the following code:

    ```
    class users::<GITHUB USERNAME> {
      govuk_user { '<GITHUB USERNAME>':
        fullname => 'FIRSTNAME LASTNAME',
        email    => 'GITHUB EMAIL',
        ssh_key  => '<SSH-PUBLIC-KEY-VALUE>',
      }
    }
    ```

    Enter your GitHub information and SSH public key value into the file. For example:

    ```
    class users::<johnsmith> {
      govuk_user { '<johnsmith>':
        fullname => 'John Smith',
        email    => 'john.smith@digital.cabinet-office.gov.uk',
        ssh_key  => '<SSH-PUBLIC-KEY-VALUE>',
      }
    }
    ```

1. Add the name of your user manifest file (`<FIRSTNAMELASTNAME>.pp`) into the list of `users::usernames` in [`hieradata_aws/integration.yaml`][integration-aws-hiera].

1. Create a pull request with these changes.

    Once the pull request has been [reviewed][merging], you can merge it and the pull request will automatically deploy to the integration environment.

    Your tech lead or any other developer with access can review this pull request.

[govuk-puppet]: https://github.com/alphagov/govuk-puppet
[integration-aws-hiera]: https://github.com/alphagov/govuk-puppet/blob/master/hieradata_aws/integration.yaml
[merging]: /manual/merge-pr.html

### Access remote environments and server

Once your pull request with your user manifest file is merged and deployed, you should test your SSH access to remote environments and servers.

If you are outside of the office or on GovWiFi, you must first [connect to the GDS VPN](#4-connect-to-the-gds-vpn).

Test your SSH access by running:

```bash
gds govuk connect --environment integration ssh backend
```

#### Running a console

Once you have SSH access into a remote environment or server, you can also open a Rails app console for a particular application so you can run commands.

For example, to open a console for GOV.UK Publisher, run the following on a `backend` machine:

```bash
$ govuk_app_console publisher
```

As a shortcut, to remove the need to look up the machine class for an application, you can use the following without SSHing first:

```bash
gds govuk connect --environment integration app-console publisher
```

## 7. Get AWS access

GDS maintains a central account for AWS access.

You must have access to this GDS account to work with [govuk-aws](https://github.com/alphagov/govuk-aws) and [govuk-aws-data](https://github.com/alphagov/govuk-aws-data).

### Request access to the GDS AWS account

You request access to the GDS AWS account through the [Request an AWS account form](https://gds-request-an-aws-account.cloudapps.digital).

Select __Request user access__ to request access to the GDS AWS account and complete the form.

After submitting the form, you should receive an email to say your account creation is in progress, and later another email saying the work has been completed.

### Sign in to AWS

To sign in, go to [the `gds-users` AWS console][gds-users-aws-signin], and enter:

- `gds-users` in __Account ID or alias__
- your `@digital.cabinet-office.gov.uk` email address in __Username__
- your AWS account password in __Password__

### Set up Multi-Factor Authentication

You must set up [Multi-Factor Authentication (MFA)][MFA] to access AWS.

How you set up MFA depends on whether you have a GDS-issued Yubikey or not.

If you have a Yubikey, follow the [Reliability Engineering documentation on using YubiKey for 2FA in Amazon Web Services](https://re-team-manual.cloudapps.digital/yubikeys.html#use-yubikey-for-2fa-in-amazon-web-services).

If you do not have a Yubikey, use the following instructions to set up your MFA device.

1. Sign in to the [`gds-users` AWS console][gds-users-aws-signin].
1. Select the __IAM__ service.
1. Select __Users__ in the left hand menu and enter your name.
1. Select the link for your email address.
1. Select the __Security credentials__ tab.
1. Select __Manage__, which is next to __Assigned MFA device__.
1. Follow the instructions to set up your MFA device.

### Generate a pair of access keys

You must generate an AWS access key ID and secret access key to be able to perform operations with AWS on the command line.

1. Sign in to the [`gds-users` AWS console][gds-users-aws-signin].
1. Select your email address in the top right of the screen.
1. Select __My Security Credentials__.
1. Select __Create access key__.
1. Make a note of your AWS access key ID and secret access key for when you [access AWS for the first time](#8-access-aws-for-the-first-time).

[gds-users-aws-signin]: https://gds-users.signin.aws.amazon.com/console

### Get access to integration infrastructure

You must get access to the integration infrastructure so you can deploy to integration.

To get access, you must add your email address to the list of `role_user_user_arns` users in the `govuk-aws-data` GitHub repo.

Open a pull request to add the following code to the `role_user_user_arns` section in the [`infra-security/integration/common.tfvars` file in the `govuk-aws-data` repo](https://github.com/alphagov/govuk-aws-data/blob/master/data/infra-security/integration/common.tfvars).

```
arn:aws:iam::<account-ID>:user/<firstname.lastname>@digital.cabinet-office.gov.uk
```

Use the same `<account-ID>` as other entries in the `role_user_user_arns` section.

See this [example pull request](https://github.com/alphagov/govuk-aws-data/pull/758/files) for more information.

After your pull request has been merged, someone from the `govuk-administrators` group or `govuk-internal-administrators` group needs to deploy the `infra-security` project. Check with your tech lead who this is.

See the [AWS IAM users documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) for more information.

## 8. Access AWS for the first time

1. Open the [GDS CLI](#3-install-gds-command-line-tools) and run `gds aws govuk-integration-poweruser -l` to open the AWS console in your web browser.
1. In the GDS CLI, enter your [AWS access key ID and secret access key](#generate-a-pair-of-access-keys).
1. Enter your [MFA token](#set-up-multi-factor-authentication) in the command line.

    Here is an example of the output you'll see:

    ```shell
    $ gds aws govuk-integration-poweruser -l
    Welcome to the GDS CLI! We will now store your AWS credentials in the keychain using aws-vault.
    Enter Access Key ID: <YOUR-ACCESS-KEY-ID>
    Enter Secret Access Key: <YOUR-SECRET-ACCESS-KEY>
    Added credentials to profile "gds-users" in vault
    Successfully initialised gds-cli
    Enter token for arn:aws:iam::123456789012:mfa/firstname.lastname@digital.cabinet-office.gov.uk: 123456
    ```

1. When prompted, save credentials to your Mac's keychain as `aws-vault` and set a password for the keychain. Save that password somewhere safe, for example in a password manager.

If you have a GDS-issued Yubikey, you can run `gds config yubikey true` in the GDS CLI to set GDS CLI to automatically pull the MFA code from your Yubikey.

You have completed the get started process.

### Reset your AWS vault password

If you forget your `aws-vault` password, you must reset that password.

1. Delete the `aws-vault` keychain by running `rm ~/Library/Keychains/aws-vault.keychain-db` in the command line.
1. Re-initialise the `gds-cli` by opening `~/.gds/config.yml` and changing `initialised: true` to `initialised: false`.

## Supporting information

Now you have completed the get started process, you should look at the following supporting information:

- the [architectural deep dive of GOV.UK][architectural-deep-dive]
- [how GDS uses Docker](/manual/intro-to-docker.html)

[architectural-deep-dive]: /manual/architecture-deep-dive.html
[govuk-aws-data-users-group]: /manual/set-up-aws-account.html#4-get-the-appropriate-access
[infra-terra]: https://github.com/alphagov/govuk-aws-data/tree/master/data/infra-security
[MFA]: https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#multi-factor-authentication
[iam]: https://console.aws.amazon.com/iam/home?region=eu-west-1#/users
