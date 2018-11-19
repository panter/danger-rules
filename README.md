[![pipeline status](https://git.panter.ch/open-source/danger-rules/badges/master/pipeline.svg)](https://git.panter.ch/open-source/danger-rules/commits/master)

# Rules for [Danger](https://danger.systems/ruby/)

Provides rules for [Danger](https://danger.systems/ruby/) that ~~can~~ should
:smile: be included in your project.

## Usage

1. Add a `Dangerfile` with the desired rules.

    ```ruby
    # imports all rules
    danger.import_dangerfile(gitlab: 'open-source/danger-rules', path: 'all')

    # only import one specific rule
    danger.import_dangerfile(gitlab: 'open-source/danger-rules', path: 'git/wip_commit')
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

The groups `panter` and `open-source` on git.panter.ch already have the
`DANGER_GITLAB_API_TOKEN` variable set. If you need to create a project within
another group you need to add the following variable to either the group or the
project.

1. Add a [Secret
   Variable](https://gitlab.com/help/ci/variables/README#variables)
   `DANGER_GITLAB_API_TOKEN` with the value from `pass -c git.panter.ch/DANGER_GITLAB_API_TOKEN`
   to your GitLab project
