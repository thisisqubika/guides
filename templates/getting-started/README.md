## Templates

This is a guide that will help you getting started with the way that README.md is written for the
different templates. This will include some required sections and some sections that could be
included optionally (in case the template requires more information)

The first section that is going to be included in the README.md is the `Getting Started`, for
that section we are going to use a "h1" in markdown is represented as `#` so the section is going
to look like this:

```
# Getting Started
```

This section is going to be divided into multiple sections, the first part is going to include some
commands about how to developers should start the project. If applicable it should include a
`Start project with docker (recommended)` and a `Start project manually` (both of them are consider
"h2" so are going to be represented with `##`)

```
## Start project with docker (recommended)

// Content

## Start project manually

// Content
```

For the manual project start up it's also required the tool installation like (Version manager, and
the different tools that the project might require node for example).

```
### Install tools

#### Version manager

// Content on how to install the version manager

#### Node.js

// Content on how to install node.js using the version manager

#### Yarn

// Content on how to install yarn using the version manager
```

After tool installation a project dependencies installation section is required as shown below

```
### Install project dependencies

// How to install each project dependency
```

Finally commands to start the project itself

```
### Start the project

// Content on how to start the project
```

If the template has additional tools that are only required for some flows (like Storybook)
add a section on that topic like this

```
## Storybook

// Explain what is storybook or the additional software and some links to their site itself,
// also any required step to run it locally
```

TODO: continue with linter and tests
