# cloudcustodianpolicies

Cloud Custodian policies and Code Build spec files for invoking Cloud Custodian. Must include Account ID in the name, e.g., 'policies-<account-id>.yml'


- name: aws-cloudtrail-not-enabled
  resource: account
  description: |
    Accounts which do not have CloudTrails
    enabled.
  mode:
    type: periodic
    schedule: "rate(1 day)"
    role: SOME_ROLE_PATCHED_BY_BUILD_SPEC
  filters:
    - type: check-cloudtrail
      global-events: true
      multi-region: true
      running: true
      kms: true
      file-digest: true
  actions:
    - type: notify
      to: []
      transport:
        type: sns
        topic: SOME_NOTIFICATION_TOPIC_PATCHED_BY_BUILD_SPEC

- name: aws-config-not-enabled
  resource: account
  mode:
    type: periodic
    schedule: "rate(1 day)"
    role: SOME_ROLE_PATCHED_BY_BUILD_SPEC
  description: |
    Accounts which do not have
    AWS config enabled.
  filters:
    - type: check-config
      all-resources: true
      global-resources: false
      running: true
  actions:
    - type: notify
      to: []
      transport:
        type: sns
        topic: SOME_NOTIFICATION_TOPIC_PATCHED_BY_BUILD_SPEC