# Troubleshoot your setup

## Windows installation

On Windows, if the error `"MSBUILD : error MSB4132"` occurs, the
[windows-build-tools](https://www.npmjs.com/package/windows-build-tools) module
might be missing. In a PowerShell window with administrator permissions, enter
the following code, and then press Enter.

```sh
npm install --global windows-build-tools
```

## Payload

To verify the payload package created by the publish script, find the skill
payload at `<your_project_path>/.deploy/{your-ask-skill-directory-name}`.

To build the payload without performing a deployment, open a command prompt,
enter the following code, and then press Enter.

```sh
npx alexa-sfb build <your_project_path>
```

## New package version

When you pull a new version of the Skill Flow Builder package to an existing
project, clean the workspace to apply the changes to your project.

1. Open a command prompt, enter `npm install`/`yarn install` and then press Enter.
2. To reinstall `alexa-sfb` enter the following commands.

```sh
cd <your_project_path>
npm upgrade # (or 'yarn upgrade' if using yarn)
```

Alternatively, if you installed the Skill Flow Builder CLI globally, complete
the following procedure.

- Open a command prompt, and enter the following command, and then replace {SFB} with the full path.  (You can find where `alexa-sfb` is installed by running `which alexa-sfb` in the command line)

```sh
npm --global upgrade {SFB}/lib/moduleSrc/alexa-sfb
```

or if using `yarn`

```sh
yarn global upgrade {SFB}/lib/moduleSrc/alexa-sfb
```

## Upgrading from ASK CLI v1

If you previously deployed a skill using ASK CLI v1 (using the SFB CLI v1), then
you will see the following message when you run `alexa-sfb deploy`:

> It seems that your project was built using ask-cli@1.x.x, which is no longer supported by Skill Flow Builder.
> Do you want to start deploying your project with ask-cli@2.x.x ?
> Your existing build artifacts will be modified to work with ask-cli@2.x.x.
> [Y/n]:

Simply enter 'Y' into the command prompt and press enter/return to continue with
the deployment. The SFB CLI v2 will run the `ask util upgrade-project` command
to upgrade the build artifacts in the `.deploy/` directory automatically and
continue with the deployment.

If you do not wish to upgrade at this time, enter 'n' instead and the deployment
will stop without making any changes to your `.deploy/` or `metadata/` directories.

For more information on the upgrade process, please see
[ASK CLI v1 to v2 Migration Guide](https://developer.amazon.com/en-US/docs/alexa/smapi/ask-cli-v1-to-v2-migration-guide.html)

## Audio mixing fails in the skill

If you are building and installing the `sfb-cli` package directly from this
repository (rather than installing via `npm`), and your skill fails when it
attempts to mix audio, it is likely your `sfb-cli` package is missing the
`ffmpeg` binary.

When building from source, you need to supply your own Linux build of FFmpeg
(including LAME) to copy into the skill. The
`scripts/setup/InstallDependenciesLinux.sh` script supplied with repository
will build FFmpeg and LAME binaries with the required configuration.

Next, copy the `ffmpeg` binary into one of these locations:

1. The `sfb-cli` package directory
2. The `code/` directory in your SFB story package

Alternatively, you can set the `ffmpeg-location-for-skill` property
in your project's `abcConfig.json` file to the full filepath for the
`ffmpeg` binary.
