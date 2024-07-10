# VIP Public Sites

This is the codebase for the public.wpvip.com multisite. Notably, the two public-facing network sites (wpvip.com and parse.ly) live here. VIP Engineers work in this repository, as well as engineers from external agencies.

Looking for guidance on developing on the WordPress VIP Platform? Head to our [documentation site](https://docs.wpvip.com/) to learn more.

## Usage

All of the directories in this repo are required, and will be available on production web servers. Any additional directories created will not be available in production.

## Support

If you need help with anything, our wider support team is [just a ticket away](https://docs.wpvip.com/technical-references/vip-support/).

## Development Workflow

Each team has their own development environment, and code is pushed from there to a unified preprod environment for testing before deployment to production.

The [GitHub pull request](https://docs.wpvip.com/technical-references/development-workflow/github-pr-reviews/) workflow should be followed for each deployment.

Each team can [sync data from production to their non-production environment](https://docs.wpvip.com/technical-references/vip-dashboard/data-sync/) at will. However, notify all teams affected by a data sync beforehand.

### Branch naming convention

Branch names should identify the team, user (optional), and task, as well as a synopsis of the change to be made. Follow this format:

`{team}/{username} - {task-id} - {description}`

Where `{task-id}` is the identifier of a task in whichever task-management tool is being used (e.g. JIRA), if there is one.

For example:
`vip/trogdor - HSR-1999 - update plugin`

### Code review

Feel welcome to request code review or feedback on ideas from the VIP Engineers. If the issue is not time dependent you may request a review directly in Github.

Also feel welcome to ask any questions about the platform, or share ideas about code-architecture. If we don't know the answer we will happily reach out to the wider team for answers.

### Best practices

To help reduce code conflicts when working with multiple teams on the same repository:

- Branches of merged PRs will be auto-deleted. Deleted branches can be recovered.
- Avoid merging incomplete or in-progress code to the `preprod` or `develop` branches, since it can cause code conflicts down the line.
- Ideally there should be regular releases to `preprod` for testing.
- Please pull (rebase) from the `master` branch regularly to keep your `develop` branch up to date.
- Any changes outside of a theme directory should be made with caution. Those are places most likely to impact running code and other teams' work.

## Local Development - Getting Started

Documentation: https://docs.wpvip.com/technical-references/vip-local-development-environment/

### Notes

- You will need a user account to [access to the VIP Dashboard](https://docs.wpvip.com/technical-references/vip-dashboard/vip-dashboard-log-in/) for this application (1513). Let us know if you need us to create one for you.
- To successfully create a VIP Local Development Environment, be sure to review and complete all of the prerequisites listed in the documentation:
  https://docs.wpvip.com/how-tos/local-development/use-the-vip-local-development-environment/

## Prerequisites

Ensure you have the following installed on your machine:
- Node.js and npm
- VIP CLI (`npm install -g @automattic/vip`)

### Setup environment

1. Clone the [vip-wordpress-com](https://github.com/wpcomvip/vip-wordpress-com) repository to an arbitrary folder on your local machine.
2. Create a local environment by running the command below. A command line setup wizard will prompt you to select a version of WordPress (select the most recent), and source your application code from your locally cloned code repository (select "Custom" and enter the absolute local path to the repository on your local).

```
vip dev-env create --slug=example-slug  // example-slug change to whatever you want
```
## Prompts and Responses

- **WordPress site title**: Enter the desired title for your WordPress site.
- **Multisite (y/N)**: Enter `y` for Multisite.
- **PHP version to use**: Enter `latest version`.
- **WordPress version**: Enter `latest version`.
- **How would you like to source vip-go-mu-plugins**: Press `Enter` to accept the default value `Demo`. This will automatically load the staging branch of VIP MU plugins on the created local environment.
- **How would you like to source application-code**: Use the `Down Arrow` to highlight the `Demo` option and press `Enter` to proceed to the next option.
- **Enable Elasticsearch (needed by Enterprise Search)? (y/N)**: Press `Enter` to accept the default value `y` to enable this
- **phpMyAdmin**: Enter `yes`.
- **XDebug**: Enter `yes`.
- **Enable Mailpit (y/N)**: Press `Enter` to accept the default value `true`.
- **Enable Photon (y/N)**: Press `Enter` to accept the default value `true`.

3. Start the environment: `vip dev-env start --slug=example-slug`

# Building Assets

To build the assets for the theme plugins, follow these steps:

## 1. Athletic Blocks (Full Site Editing Block)

1. **Navigate to the project directory:**

    ```sh
    cd themes/vip-valet/theme-plugin/athletics-blocks
    ```

2. **If there is an existing `build` folder, delete it:**

    ```sh
    rm -rf build
    ```

3. **Delete the `package-lock.json` file:**

    ```sh
    rm package-lock.json
    ```

4. **Install modules:**

    ```sh
    npm install
    ```

    or

    ```sh
    npm install --force
    ```

5. **Run the build process:**

    ```sh
    npm run build
    ```

## 2. VIP Valet Theme

1. **Navigate to the theme directory:**

    ```sh
    cd themes/vip-valet
    ```

2. **Delete the `package-lock.json` file:**

    ```sh
    rm package-lock.json
    ```

3. **Install modules:**

    ```sh
    npm install
    ```

    or

    ```sh
    npm install --force
    ```

4. **Run the build process:**

    ```sh
    npm run build
    ```

## 3. VIP Marketing Theme

1. **Navigate to the theme directory:**

    ```sh
    cd themes/vip-marketing
    ```

2. **Delete the `package-lock.json` file:**

    ```sh
    rm package-lock.json
    ```

3. **Install modules:**

    ```sh
    npm install
    ```

    or

    ```sh
    npm install --force
    ```

4. **Run the build process:**

    ```sh
    npm run build
    ```

## Updating Development Environment

Whenever you make any changes to the environment, run the following command:

```sh
vip dev-env update --app-code=path-to-your-cloned-repo --slug=example-slug
```

### Install dependencies

1. **PHP/Composer** - In order to run the build script successfully we require your machine to have php and composer installed on it.
   * MacOS: The preferred install method is via Homebrew. https://brew.sh/
     * You can skip this command if Homebrew is already installed:
       *  `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
       * `brew install php composer`
2. Run `bash ./scripts/setup-local.sh` to install dependencies and build initial static assets for all projects within this multisite.
   * The script runs a series of build commands for multiples themes and plugins. You can optionally use the script as a reference and selectively run commands to build dependencies only for the themes or plugins you need for your development. 

### Import database

1. [Download a database backup](https://dashboard.wpvip.com/apps/1513/production/data/database/backups) from the VIP Dashboard.
3. Run the following command, with `<local_sql_file>` replaced by the location of the database file on your local machine:

```
vip --slug=wpvip dev-env import sql <local_sql_file> --search-replace="public.wpvip.com,public.wpvip.vipdev.lndo.site" --search-replace="wpvip.com,wpvip.vipdev.lndo.site" --skip-validate
```


#### Import Script

There is a script `/scripts/import-db-local.sh` which can help replacement for all multisite domains. Before running it be sure to double check that the `LANDO_SUFFIX`, `SEARCH`, & `REPLACE` domains match the domains of the current source DB setup, and your local domain structure.

### Finishing up

1. Jump to the "Project Instructions" section below for additional steps depending on the project you're working on.
2. Visit the HTTPS NGINX URL (displayed after you start the environment) to find the network you would like to work with. (e.g. https://wpvip.vipdev.lndo.site)

### Adding User / Super Admin

If you're not already a user on the site or you need super admin locally, these commands can help you get access in your local instance.

#### Add a local instance user:

Note your wpvip lando domain from the end of the output when running the start command. If you are following the instructions from above this should be `wpvip.vipdev.lndo.site`.


Your password will be generated as output of the command.

```
vip dev-env --slug="wpvip" exec -- wp user create <username> <email@email.com> --role=administrator --url="<site.domain.lndo.site>"
```

#### WP-CLI & Super Admin

You will need a Super Admin role to have full access to plugins, themes, and some other settings on the multisite network.

```
vip dev-env --slug="wpvip" exec -- wp super-admin add <username>
```

Other WP-CLI commands can be run in the same format. See https://developer.wordpress.org/cli/commands/ for details.

```
vip dev-env --slug="wpvip" exec -- wp <command>
```

### Troubleshooting

#### I'm not able to view the site over HTTPS

Additional headers might be needed in the `vip-config.php` file:

```
<?php
if ( $_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https' ) {
  $_SERVER['HTTPS'] = 'on';
  $_SERVER['SERVER_PORT'] = 443;
}
```

#### The setup instructions aren't working for me

Things to try:

- Do you have the latest XCODE CLI Installed? `xcode-select --install`
- Did you choose the same version of PHP that the applicaiton is running?
- If you are missing styles, have you run all the `npm build` steps necessary for that network site's theme per the project instructions below?
- If you get a white page on the front end, have you looked in the error logs?
  - If you can login to the admin you can see the fatal errors on the front-end UI.

### Project Instructions

**wpvip.com**

- You'll need to run `npm run start` in this directory to generate the static assets needed in a local environment.

### Updating the embedded Valet parent theme

The Valet theme is set up as a sub-tree in themes/vip-valet directory. Child themes power various subsites. To update the parent site you need to pull changes from the sub-tree.

1. Add a remote for the `vip-valet-theme` repo in the `vip-wordpress-com` repo on your machine. In the root directory of the `vip-wordpress-com` repo, run: `git remote add vip-valet-theme git@github.com:Automattic/vip-valet-theme.git`
2. Fetch changes from the `vip-valet-theme` repo. In the root directory of the `vip-wordpress-com` repo, run: `git fetch vip-valet-theme trunk`
3. Update the subtree. In the root directory of the `vip-wordpress-com` repo, run: `git subtree pull --prefix themes/vip-valet vip-valet-theme trunk --squash -m "update valet parent theme"`
4. That adds a commit which you can then push to the vip repo.

### Making changes to the embedded Valet parent theme

The valet theme is set up as a sub-tree. This means you can make changes to the master theme in this repo and push the changes upstream to the remote repo as a new branch. This is useful when adding features to a site using the Valet theme. You can develop the theme locally, in context and then push changes to the master for a PR.

To push changes upstream as a new PR run this commend: `git subtree push --prefix themes/vip-valet vip-valet-theme <new branch name>`
