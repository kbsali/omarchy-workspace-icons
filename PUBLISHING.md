# Publishing to the Omarchy plugin marketplace

Requirements (https://github.com/omacom/omarchy-plugin-marketplace/blob/main/SUBMISSION.md):
public GitHub repo with `manifest.json` at the root, README with install and removal
instructions, LICENSE, documented dependencies, optional root `preview.png`, globally unique
plugin id. `tjcelaya.workspace-icons` was unused in the catalog
(https://plugins.omarchy.org/catalog.json) as of 2026-09-30.

## Resume here on 2026-10-11 (the 11th), before anything else is published

v0.3.0 (middle-click a panel row to close its window or stop its agent) was tagged and
pushed at 2026-10-11 00:15 EDT. Left for later on the 11th:

1. Smoke-test the real install path (below) against the pushed `main`.
2. Promote the release through the marketplace "Plugin verification" form with the full
   SHA of `v0.3.0`: `4cb447c9b6082c62e45007fc1f801fe3f2faed47`.
3. Delete this section.

Checks before every submission or release:

    omarchy plugin validate ~/src/tjcelaya/omarchy-workspace-icons
    T=$(mktemp -d) && git clone -q . $T && omarchy plugin validate $T && rm -rf $T
    jq . manifest.json >/dev/null

Release: bump `version` in `manifest.json`, commit, `git tag vX.Y.Z && git push --tags`.
`omarchy plugin update` pulls HEAD, so keep `main` releasable.

Smoke-test the real install path:

    ./uninstall.sh
    omarchy plugin add https://github.com/tjcelaya/omarchy-workspace-icons.git --enable
    omarchy plugin disable omarchy.workspaces
    # then back to the dev symlink:
    omarchy plugin remove tjcelaya.workspace-icons --yes && ./install.sh

Submitted 2026-10-06 as https://github.com/omacom/omarchy-plugin-marketplace/issues/10211.
Editing the issue body re-runs validation against the current HEAD of main.

Submission is a GitHub issue on omacom/omarchy-plugin-marketplace titled
`[Plugin]: Workspace icons` with the body in `submission.md` (six headings in order, all five
checklist boxes ticked). The form:
https://github.com/omacom/omarchy-plugin-marketplace/issues/new?template=submit-plugin.yml
or `gh issue create --repo omacom/omarchy-plugin-marketplace --title "[Plugin]: Workspace icons" --body-file submission.md`.

Their CI validates the commit that is HEAD at submission time; a maintainer then applies
`approved-and-verified`. Later releases are promoted through the "Plugin verification" form
with the new full 40-char SHA, not by resubmitting.

Similar listings to be aware of: `saif.workspaces`, `lobo.omaspaces`,
`io.github.thetrueferret.decent-workspaces`, `murdi.named-workspaces`. This plugin's
distinguishing points are the program-inside-terminal detection and Omarchy webapp icon
matching; keep them prominent in the README.
