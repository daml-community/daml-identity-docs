# Approving Onboarding
The current security policy dictates that adding keys must be approved by 2 or more **maintainers**. The process of adding keys is as follows:
1. An organisation opens a PR containing their changes and requests a review from **maintainers**
2. **Maintainers** then review the PR and deny/approve. Two votes are required. Each GitHub PR approval is converted into a `gittuf` approval via the [`gittuf` GitHub app](https://github.com/gittuf/github-app).
3. Once the threshold of votes is met, the PR can then be merged by a **maintainer**
