# Setup Flutter submodule

This action installs and configures Flutter from a git submodule.

It largely does the same as other Flutter setup actions,
but it uses a git submodule to get Flutter instead of downloading the latest SDK archive from Google.<br>
This ensures your CI builds always use exactly the same Flutter version as your submodule, helping to make your builds more reproducible.

## Usage

### Create the submodule

In your project, add Flutter as a git submodule.
The following command adds Flutter to a `submodules/flutter` directory and checks out the `stable` branch.

```bash
git submodule add -b stable https://github.com/flutter/flutter.git submodules/flutter
```

### Use the action

Then, in your GitHub Actions workflow, use the action like this:

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Setup Flutter submodule
    uses: adil192/setup-flutter-submodule@v1
    with:
      flutter-path: submodules/flutter

  - name: Build for Android (or whatever you want to do with Flutter)
    run: flutter build apk
```

## Options

```yaml
  - name: Setup Flutter submodule
    uses: adil192/setup-flutter-submodule@v1
    with:
      # The path to the Flutter submodule (absolute or relative).
      flutter-path: submodules/flutter

      # (Optional)
      # The path to the pub cache directory (absolute or relative).
      # Defaults to .pub-cache in the current working directory.
      pub-cache: .pub-cache

      # (Optional)
      # Whether to cache the Flutter SDK between workflow runs.
      # Defaults to true.
      cache-flutter: true

      # (Optional)
      # Whether to cache the pub cache between workflow runs.
      # Defaults to true.
      cache-pub: true

      # (Optional)
      # Whether to only restore caches and not save them.
      # Defaults to false.
      no-save-caches: false
```

## Updating Flutter

Git submodules do not automatically update. When a new stable Flutter version is released, you need to manually update the submodule in your repository with the following command:

```bash
# Update the submodule from the remote repository
git submodule update -f --remote submodules/flutter
# Then commit the change
git commit -m "chore: update flutter submodule"
```

Flutter recommends that you subscribe to the
[flutter-announce](https://groups.google.com/forum/#!forum/flutter-announce)
group to be notified of new Flutter releases. [\[source\]](https://github.com/flutter/flutter/blob/stable/CHANGELOG.md)
