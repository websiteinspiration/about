# About Sourcegraph

This repository contains information and documentation about [Sourcegraph](https://sourcegraph.com):

<!--
TODO(sqs):
projects -> product
         -> company
website/docs -> docs
blog -> blog
website -> website

@farhan hey, can I get your take on the direction for the about repo? I want to start using it for more things (product roadmap, company handbook, etc.) and want to see how best to do that. you free for a phone chat?

I am going to reorganize the `about` repository with the goal of making the directory hierarchy optimzed for browsing/reading instead of for generating the site. 


---

# How to build and release things at Sourcegraph

To build and release a significant product improvement or feature, there are 4 steps:

1. Plan
2. Work
3. Ship
4. Release

The goals of this process are to: 

- Make it clear what's shipping and when
- Make it easy for anyone, especially users and customers, to give feedback about upcoming features
- Eliminate blockers to shipping that are outside the project team's control

## 1. Plan the project

A project should be planned when: 

- it is necessary to start work on it; or
- uncertainty around it makes it hard to accomplish other things now.

Anyone can start planning a project. @sqs will make sure projects on the [product roadmap](TODO) are planned.

To plan a project:

1. Schedule a planning meeting (with a Zoom video call) with the project team and @sqs.
   - Mention it in #product.
   - Avoid pre-discussing the discussion. Anyone with questions/concerns should join the meeting or submit questions to be addressed during the meeting.
1. During the planning meeting:
   1. Create a new branch on the sourcegraph/about repository with the project name (e.g., `mobile-friendly-search`).
   1. Make changes on the branch (as needed):
      - Write the draft blog post (in [`./blog`](./blog)) that will be published when the project ships.
      - Update documentation (in [`./docs`])(./docs)).
      - Update website content, including the homepage and product feature list.
      - Update the product roadmap.
      - Update the release plan for the first release after the project is expected to ship.
	  - Add placeholder `TODO($USER)`s for things that can't be determined until starting work.
   1. Submit a PR (the **project plan PR**) for the branch with the project's name as the title and with the `project` label.
1. Immediately after the planning meeting:
   - Post the PR link in #product.
1. Wait for the project to be approved (by @sqs adding the `approved` label to the PR).
   - Usually this will happen in the planning meeting itself.

(Note that there is no separate "project plan" beyond the actually useful documents about the project, such as the blog post and docs. This is intentional.)

## 2. Work on the project

The project team is now responsible for:

- Making come true the the new or changed content in the project plan PR.
- Addressing all TODOs in the project plan PR.

Basically, designing and implementing the feature. Examples:

> If the project PR adds a doc page saying "You can do X", then the project team needs to make it so users can do X.

> If the project PR contains a `TODO(alice): make sure this works for deployments on both Docker and k8s`, then Alice needs to make sure it works and remove the TODO. If that's not possible, she needs to raise the issue to the project team (or @sqs, if the project team can't solve the problem).

While working:

- Communicate: The project plan PR is the source of truth for what the project is, so keep it reasonably up to date while working on the project.
- Be explicit about whose help is needed: If anyone outside the project team needs to do a lot of work related to the project (except @sqs), that person must be explicitly added to the project team to make priorities and responsibilities clear.

## 3. Ship the project

To ship the project:

1. The project team merges the project plan PR.

This causes the blog, documentation, website, and product roadmap to be updated immediately.

Obviously, if there are any remaining `TODOs`s in the 

## 4. Include the project in a release

No action necessary by the project team. The release captain uses the release plan for the project that was previously merged.

When the release is tagged in sourcegraph/sourcegraph and sourcegraph/enterprise, the release captain also tags the sourcegraph/about repository to snapshot the docs for the release tag. The docs shown on [about.sourcegraph.com](https://about.sourcegraph.com) are always from `master`.

-->

- [Product](./product) -- features,  to add a new product feature, improve UX, etc.
- [Documentation](./website/docs) -- 
- [Blog](./blogposts) -- source files for posts on the [Sourcegraph Blog](https://about.sourcegraph.com/blog)
- [Website](./website) -- sources for [about.sourcegraph.com](https://about.sourcegraph.com)

**Caution: This repository contains the future.** Because Sourcegraph is open source and open product, you'll see docs about future, not-yet-shipped product features. Check the publish/release date in the Markdown file header to be sure.

## Contributing

This repository is public so that everyone can see and influence Sourcegraph's product direction. We welcome your input!

- **Feedback or questions** on projects in our [product roadmap](./projects): [start a discussion](TODO) on any of the [`projects/*`](./projects) files, or [file an issue](https://github.com/sourcegraph/about/issues).
- **Feature requests:** file an issue on the most relevant repository (e.g., [sourcegraph/sourcegraph](https://github.com/sourcegraph/sourcegraph) or [sourcegraph/browser-extensions](https://github.com/sourcegraph/sourcegraph)) asking for the feature.
- **Project proposals:** if you want to contribute a detailed project proposal for a new feature (instead of just requesting the feature), [submit a PR to this repository](./projects/README.md).
