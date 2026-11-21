# Repository rule

Keep this platform reusable and portable. Builds, releases, deployments, tests, and assessments must not depend on a personal laptop, `secure-infra-worker`, or hand-installed state; declare all required tools, inputs, and setup in the repository so each process can be reproduced from a fresh GitHub-hosted runner or another cloud environment. Engineering VMs are for interactive development only, never hidden release infrastructure.

# Ask before adding

Before adding any new mechanism, layer, tool, binding, lock or file, tell the owner what it is for and what it will cost on every future change, and wait for her yes. Do not add it because it is a good practice in general. This project was archived because many such additions, each reasonable alone, made it too rigid to maintain.
