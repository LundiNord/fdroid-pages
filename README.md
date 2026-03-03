# fdroid-pages

Open https://fdroid.nyxnord.de/fdroid/repo/ to see the repo.

## Using this repo as a template

> [!IMPORTANT]
> If you want to [create your own repo](https://f-droid.org/en/docs/Setup_an_F-Droid_App_Repo/) and use this repo as a base, make sure to remove/customize all assets like images, links and names!

You need a few secrets that are used for verifying the repo.

To generate these, you have to run the ``fdroid-repo-generator``-Docker image offline.

0. Ensure that you have Docker installed
1. Check out the repo and open it in your terminal
2. Select the directory [``fdroid-repo-generator``](e./fdroid-repo-generator) using ``cd fdroid-repo-generator``
3. Build the image offline: ``docker build --tag fdroid-repo-generator .`` (the dot at the end is important)
4. Launch the container ``docker run --rm -it --entrypoint=/bin/bash -v %cd%/temp-repo:/repo -w /repo fdroid-repo-generator``
    * This will create a new directory ``temp-repo``, if this is already present you may need to clean it
    * After the container is launched, a console should be visible
5. Execute ``fdroid init`` to initialize the repo
6. When this is done exit the container with typing ``exit``

You now should have a fresh F-Droid repo initialized in ``temp-repo``.

The next step is to get the following secrets and store them in GitHub Action secrets:

| What? | Secret name | Notes |
| --- | --- | --- |
| ``keystore.p12`` | ``KEYSTORE_BASE64`` | Needs to be converted to base64.<br/>Easiest way: ``cat keystore.p12 \| base64 > keystore_b64.txt`` |
| ``config.yml``→``repo_keyalias`` | ``REPO_KEYALIAS`` | |
| ``config.yml``→``keystorepass`` or ``keypass`` (identical) | ``KEYPASS`` | ``keystorepass`` and ``keypass`` are usually identical |
| ``config.yml``→``keydname`` | ``KEYDNAME`` | |

## Updating
Updates are checked for every night [using GitHub Actions](./.github/workflows/update.yml).
If an update is found, the repo is rebuilt.
