[![pipeline status](https://git.panter.ch/open-source/danger-rules/badges/master/pipeline.svg)](https://git.panter.ch/open-source/danger-rules/commits/master)

# Rules for [Danger](https://danger.systems/ruby/)

Provides rules for [Danger](https://danger.systems/ruby/) that ~~can~~ should
:smile: be included in your project.

## Usage

1. Add a `Dangerfile` with the desired rules.

    ```ruby
    # imports all rules
    danger.import_dangerfile(gitlab: 'open-source/danger-rules', path: 'rules/all')

    # only import one specific rule
    danger.import_dangerfile(gitlab: 'open-source/danger-rules', path: 'rules/git/wip_commit')
    ```

1. Add the following to your `.gitlab-ci.yml`

    ```yaml
    # The image needs to have ruby installed
    # You may also define a specific image for the `danger-review` job if your
    # overall image doesn't contain ruby.
    image: ruby:2.5

    variables:
      DANGER_GITLAB_HOST: git.panter.ch
      DANGER_GITLAB_API_BASE_URL: https://git.panter.ch/api/v4

    danger-review:
      allow_failure: true
      before_script:
        - gem install danger-gitlab --no-document
      only:
        variables:
        - $DANGER_GITLAB_API_TOKEN
      except:
        refs:
        - master
      script:
        - git version
        - bundle exec danger --fail-on-errors=true
    ```

The group `panter` on git.panter.ch already has the `DANGER_GITLAB_API_TOKEN`
variable set. If you need to create a project within another group you need to
add the following variable to either the group or the project.

1. Add a [Secret
   Variable](https://gitlab.com/help/ci/variables/README#variables)
   `DANGER_GITLAB_API_TOKEN` with the value from `pass -c git.panter.ch/DANGER_GITLAB_API_TOKEN`
   to your GitLab group or project

## Contributing

All `Dangerfile`s in any subdirectory starting from `src` are included.  The
`Dangerfile`s in `rules` are the generated ones (by `rake generate`). This
directory includes every single `Dangerfile` as well as `all/Dangerfile` which
includes all other existing ones.

If you want to extract common logic you may place library methods in the `lib`
directory, which will be inlined in the generated `Dangerfile`s. Requiring
anything other from `lib` (e.g. third party gem) is currently not supported.

### Create a new `Dangerfile`

1. Create a new Dangerfile with the desired rules, e.g. `git/commit_message_with_typo/Dangerfile`
1. Execute `rake generate`
1. Commit the new file and the newly generated or updated `rules/all/*/**`
