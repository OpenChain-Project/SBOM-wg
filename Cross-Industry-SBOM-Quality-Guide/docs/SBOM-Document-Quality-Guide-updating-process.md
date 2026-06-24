# SBOM Document Quality Guide Updating Process

## 1. Overview

The SBOM Document Quality Guide will be managed on GitHub going forward. Corrections to text, typos, and other content will be handled by raising Discussions, Issues and accepting Pull Requests.
This document summarizes the procedures for that process.

## 2. Processes

### 2.1 Proposals and Comments Requiring Discussion

1. A Draft Release will be announced prior to the Official Release.
2. A public comment period of approximately one month will follow. During this period, comments are accepted via mailing lists, Discussions, and similar channels. **Discussions threads will not be used for active debate during the public comment period itself.**
3. After the public comment period closes, online discussion takes place in Discussions, and responses are recorded there.
4. If a change is needed, an Issue linked to the relevant Discussions thread is raised, and a PR linked to that Issue is submitted.
5. Anyone may submit a PR, not only the person who posted the original comment.
6. All PRs must carry DCO sign-off with the following tags:  
   ```
   Reported-by:  <person who submitted the initial comment>
   Reviewed-by:  <all people who participated in the discussion>
   Signed-off-by: <person who created the PR>
   Acked-by:     <all people who reviewed the PR and agreed it is ready to merge>
   Assisted-by(OPTIONAL):   AGENT_NAME:MODEL_VERSION [TOOL1] [TOOL2]
   ```  
7. If online discussion alone is not sufficient to reach consensus, the item will be brought to the SBOM Working Group regular meeting for further discussion.
   For any Discussions item that needs to be escalated to a meeting, please click the importance reaction button on that thread. This applies both to items that have not yet reached consensus and to items you simply feel would benefit from meeting-level discussion.
   Items that accumulate a sufficiently high importance rating will be added to the meeting agenda.  
   > NOTE: the exact threshold is to be determined through WG discussion  

### 2.2 Minor Corrections Such as Typos

1. For typos and similar minor corrections, raise an Issue directly and submit a PR linked to that Issue.
2. Anyone may submit a PR, not only the person who proposed the correction.
3. All PRs must carry DCO sign-off with the following tags:  
   ```
   Reported-by:  <person who submitted the initial comment>
   Reviewed-by:  <all people who participated in the discussion>
   Signed-off-by: <person who created the PR>
   Acked-by:     <all people who reviewed the PR and agreed it is ready to merge>
   Assisted-by(OPTIONAL):   AGENT_NAME:MODEL_VERSION [TOOL1] [TOOL2]
   ```

4. If a reviewer determines that discussion is needed during the PR review, the reviewer
   creates a Discussions thread and links the Issue to that thread.

## 3. Review

Reviewers should check the relevant Discussions thread and confirm the following before merging:  
1. The PR correctly reflects the outcome of the Discussions.
2. The PR itself contains no new typos or errors.
3. DCO tags are present and correctly configured.

> Note: A GitHub Actions workflow for automated DCO checking is planned for a future iteration.

## 4. Reviewer Selection

At present, approximately three members will be designated by the SBOM WG Chair from among those who have contributed to the creation of the SBOM Document Quality Guide.
Those nominated who are willing to participate are asked to serve as reviewers.
The future selection process has not yet been determined and will be decided through discussion within the WG.

## 5. Official Release

Once all public comments have been incorporated and all corrections are complete, the guide will be formally released under Semantic Versioning. The release date will be noted beneath the version number.
The release tag will be applied with the approval of at least 2/3 of the designated Reviewers.

## 6. Acknowledgements

All contributors will be listed in the Acknowledgements section, including those who submitted public comments and those who proposed corrections.
The initial version of the SBOM Document Quality Guide was managed on Google Docs, and contributor names were recorded there. Going forward, Acknowledgements will use GitHub account names or other nicknames by default.
If you would like your real name to appear in the Acknowledgements instead, please let us know via Discussions or another appropriate channel.

## EOF
