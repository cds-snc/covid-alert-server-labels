
# Origami Labels

GitHub action to sync a repo's issue labels with the standard Origami set.


## Usage

To use this action, create the following file in your GitHub repo:

```
.github/workflows/sync-repo-labels.yml
```

```yml
on: [issues, pull_request]
jobs:
  sync-labels:
    runs-on: ubuntu-latest
    name: Sync repository labels
    steps:
      - uses: Financial-Times/origami-labels@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

:warning: Whenever an issue is opened, the workflow will execute. This overrides all labels in the repository and may result in loss of data if it's run on a repo that isn't owned by Origami.

You can do this by running the following command from a repo:

```bash
mkdir -p .github/workflows && curl https://raw.githubusercontent.com/Financial-Times/origami-labels/v1/example.yml --output .github/workflows/sync-repo-labels.yml
```


## Labels

You can find the current labels in [`./labels.js`](labels.js). Edit this file to make changes to Origami's suite of labels.

### Current Labels

Label|Description|Color|Example
-----|-----|-----|-----
blocked|Work blocked by something else|`#CC0000` (crimson)|<img width="67" alt="label-blocked" src="https://user-images.githubusercontent.com/138944/76612724-c8c00480-6514-11ea-9dee-5e2344d31864.png">
breaking|Will require a major version bump|`#990F3D` (claret)|<img width="72" alt="label-breaking" src="https://user-images.githubusercontent.com/138944/76612740-d7a6b700-6514-11ea-9cb2-ecccfa30dc6b.png">
bug|Something isn't working|`#CC0000` (crimson)|<img width="41" alt="label-bug" src="https://user-images.githubusercontent.com/138944/76612741-d7a6b700-6514-11ea-885a-567d110b9e84.png">
discussion|General discussion including support questions|`#FF7FAA` (candy)|<img width="84" alt="label-discussion" src="https://user-images.githubusercontent.com/138944/76612742-d83f4d80-6514-11ea-9a2e-1d4b3b3eec8d.png">
documentation|Improvements or additions to documentation|`#CCE6FF` (sky)|<img width="113" alt="label-documentation" src="https://user-images.githubusercontent.com/138944/76612743-d83f4d80-6514-11ea-8d29-cb9ef62751cf.png">
duplicate|This issue or pull request already exists|`#CCC1B7` (black-20)|<img width="75" alt="label-duplicate" src="https://user-images.githubusercontent.com/138944/76612745-d8d7e400-6514-11ea-83fa-66ce584708bf.png">
feature|New feature request|`#00994D` (jade)|<img width="62" alt="label-feature" src="https://user-images.githubusercontent.com/138944/76612746-d8d7e400-6514-11ea-910c-bc0a795ab7ae.png">
good starter issue|Good for newcomers|`#0D7680` (teal)|<img width="132" alt="label-good-starter-issue" src="https://user-images.githubusercontent.com/138944/76612748-d9707a80-6514-11ea-8a98-5caafa1f6fc2.png">
help wanted|We'd appreciate some help with this|`#96CC28` (wasabi)|<img width="94" alt="label-help-wanted" src="https://user-images.githubusercontent.com/138944/76747988-ef2eab80-6771-11ea-90f0-71ad15530cc9.png">
maintenance|Technical tasks that might make things better|`#FFEC1A` (lemon)|<img width="99" alt="label-maintenance" src="https://user-images.githubusercontent.com/138944/76612752-d9707a80-6514-11ea-9af3-d747bc12696d.png">
released|This work has been released and is now live|`#CCC1B7` (black-20)|<img width="71" alt="label-released" src="https://user-images.githubusercontent.com/138944/76747995-efc74200-6771-11ea-8b5b-791e5c9a71b3.png">
proposal|A proposed change which requires approval or discussion|`#262a33` (slate)|<img width="72" alt="label-proposal" src="https://user-images.githubusercontent.com/138944/76612754-da091100-6514-11ea-96b9-2602d529ea4f.png">
wontfix|This will not be worked on|`#CCC1B7` (black-20)|<img width="63" alt="label-wontfix" src="https://user-images.githubusercontent.com/138944/76612756-daa1a780-6514-11ea-8091-0509d9e94cac.png">

#### Origami-Type Labels

Origami-type labels will be automatically added to issues based on the Origami manifest contained within the repo. These map to the types defined in the [Origami Manifest Spec](https://origami.ft.com/spec/v1/manifest/#origamitype).

Label|Description|Color|Example
-----|-----|-----|-----
[origamiType]|Relates to an Origami [origamiType]|`#593380` (velvet)|<img width="62" alt="label-service" src="https://user-images.githubusercontent.com/138944/76617875-19d4f600-651f-11ea-84bb-111122ce9203.png">

#### Continuous Delivery Labels

These labels are used to automate releases.

Label|Description|Color|Example
-----|-----|-----|-----
major|Add to a PR to trigger a MAJOR version bump when merged|`#990F3D` (claret)|<img width="52" alt="label-major" src="https://user-images.githubusercontent.com/138944/76637877-6af6e100-6543-11ea-9344-27c71860030b.png">
minor|Add to a PR to trigger a MINOR version bump when merged|`#00994D` (jade)|<img width="53" alt="label-minor" src="https://user-images.githubusercontent.com/138944/76637881-6c280e00-6543-11ea-811f-3cca4bc63cf9.png">
patch|Add to a PR to trigger a PATCH version bump when merged|`#CC0000` (crimson)|<img width="53" alt="label-patch" src="https://user-images.githubusercontent.com/138944/76637883-6cc0a480-6543-11ea-8fea-11012d88a2b7.png">

### Changing a label name

When you want to change a label's name, it's very important to add the _old_ label name to the list of aliases for that label. This will ensure that the label is renamed rather than being removed then created. Failing to do this will result in loss of data.

  1. Copy the value of the `name` property and add a new entry in that label's `aliases` array
  2. Change the `name` property of the label to your new value

### Changing a label description

Change the `description` property for the label you wish to update.

### Changing a label colour

Add the new color value to the `colors` object. Change the `color` property for the label you wish to update.


## Development

Work should be based on the `master` branch, with changes PRed in.

If your changes are not breaking, merge them into the `v1` branch, and they'll be picked up by every repo running `v1` automatically.

If your changes ARE breaking, then you should create a `v2` branch based on master and update your chosen repo to use the new workflow.
