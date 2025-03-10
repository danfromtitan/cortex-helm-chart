# Changelog

## master / unreleased

* [ENHANCEMENT] Includes enable flags for each component #319

## 1.3.0 / 2022-02-10

* [CHANGE] move from quay.io/kiwigrid/k8s-sidecar to omegavvweapon/kopf-k8s-sidecar image #302
* [CHANGE] undo querier store optimization #304
  * config.querier.query_ingesters_within: 13h -> 0s (default)
  * config.querier.query_store_after: 12h -> 0s (default)
* [CHANGE] Change sidecar default values to match values in documentation #317
* [CHANGE] Remove ruler.persistentVolume.subPath since it was never persistent to begin with #317
* [ENHANCEMENT] Fix the indentation of memcached guide #309
* [ENHANCEMENT] Added api endpoints for Grafana 8 unified alerting #291
* [ENHANCEMENT] Add guide on how to configure alertmanager/ruler with configmap #313
* [DEPENDENCY] Update Helm release memcached to v5.15.14 #301

## 1.2.0 / 2021-12-29

* [CHANGE] Use port number for prometheus port annotations. #288
* [CHANGE] Disable ingester startup probes by default. #286
* [CHANGE] Moved HPA memory specification to be the first item in the list. #284
* [FEATURE] Optionally manage cortex config as configmap. #280
* [ENHANCEMENT] Upgrade to Cortex v1.11.0 #272
* [DEPENDENCY] Update Helm release memcached to v5.15.12 #274

## 1.1.0 / 2021-12-01

* [FEATURE] Allow different service accounts per dep/statefulset. #264
* [CHANGE] Enable bucket index by default #275
* [CHANGE] Disable ingester liveness probes by default. #263
* [BUGFIX] workaround for .Capabilities.APIVersions. #277
* [DEPENDENCY] Update Helm release memcached to v5.15.9 #273

## 1.0.1 / 2021-11-26

* [BUGFIX] alertmanager/ruler deployment: fix indentation #266

## 1.0.0 / 2021-11-25
### This Release includes BREAKING CHANGES


* [FEATURE] Add autoscaler for nginx #249
* [FEATURE] Add in lifecycle for querier, querier-frontend, and distributor #243
* [FEATURE] Add ingressClass to ingressResource #227
* [ENHANCEMENT] Allow overriding cortex Version #227
* [ENHANCEMENT] Update policy k8 Version #227
* [ENHANCEMENT] Improved default values for bitnami/memcached #227
* [ENHANCEMENT] Update container names to match cortex-mixin expectations #233
* [CHANGE] Deprecate *.persistence #227
* [CHANGE] Remove legacy clusterPort from alertmanager #227
* [CHANGE] Remove useless nginx.serviceMonitor section #227
* [CHANGE] Configure blocks storage engine by default instead of chunks #227
* [CHANGE] Adjusted cortex default configuration #227
  * server.grpc_server_max_recv_msg_size: 104857600 -> 10485760
  * server.grpc_server_max_send_msg_size: 104857600 -> 10485760
  * server.grpc_server_max_concurrent_streams: 1000 -> 10000
  * server.ingester_client.grpc_client_config.max_recv_msg_size: 104857600 -> 10485760
  * server.ingester_client.grpc_client_config.max_send_msg_size: 104857600 -> 10485760
  * config.limits.enforce_metric_name: false -> true
  * config.ruler.enable_api: false -> true
  * configure memberlist automatically
  * config.ingester.lifecycler.final_sleep: 0s -> 30s
  * config.querier.query_ingesters_within: 12h -> 13h
  * config.querier.query_store_after: 0 -> 12h
* [BUGFIX] Fix nil pointer evaluation when using `ruler.dictonaries` option #242
* [BUGFIX] Fixed hardcoding of containerSecurityContext in Ingester statefulset #258
* [BUGFIX] Don't create nginx resources unless nginx is enabled. #261
* [DEPENDENCY] Update Helm release memcached to v5.15.5 #241
* [DEPENDENCY] Update Helm release memcached to v5.15.8 #247

## 0.7.0 / 2021-10-05

* [FEATURE] Support runtime configuration #209
* [FEATURE] Add autoscaler for queriers #190
* [FEATURE] Add autoscaler for distributors #189
* [FEATURE] Add autoscaler for ingesters #182
* [ENHANCEMENT] Support customizing the nginx config with values #213
* [ENHANCEMENT] Upgrade to Cortex v1.10.0 #204
* [ENHANCEMENT] Populate config.querier.store_gateway_addresses automatically based on other config #201
* [ENHANCEMENT] Graceful shutdown of ingesters #195
* [ENHANCEMENT] Define namespace in templates #184
* [ENHANCEMENT] Use FQDN for memcached addresses #175
* [ENHANCEMENT] Optionally generate endpoints for `X-Scope-OrgID` injection (multi-tenancy) #180
* [ENHANCEMENT] Optionally configure Basic Auth within Nginx #205
* [ENHANCEMENT] Add extraEndpointSpec to all servicemonitors for specifying things like TLS #216
* [ENHANCEMENT] Add option to specify `podManagementPolicy` when using ingesters as statefulset #236
* [BUGFIX] Fix whitespace in runtime-config annotations, introduced in #209, fixed in #212
* [BUGFIX] Correcting nginx config for auth orgs to right proxy_pass #192
* [DEPENDENCY] Update Helm release memcached to v5.15.4 #228

## 0.6.0 / 2021-06-28

* [CHANGE] Removed dnssrvnoa resolution from block memcached (probably oversight) and moved back to simple dns resolution #164
* [CHANGE] Removed http-metrics port from every headless service and added ClusterIP service with http-metrics port to every component who before only had an headless service before #169
* [FEATURE] Support dynamic configuration of Ruler and AlertManager using sidecar #150
* [ENHANCEMENT] Enable/Disable security & container security context #158
* [ENHANCEMENT] ServiceMonitors: add options to configure metricRelabelings and relabelings #165
* [ENHANCEMENT] Support specification of whether service accounts should automount token by adding value for Cortex service account and upgrading memcached dependency to version which includes same change #142
* [BUGFIX] Fixed the default label used in pod affinity expression #162
* [BUGFIX] Fix label and annotation overrides for services (thanks @kwangil-ha) #164
* [BUGFIX] Fix store gateway service name regression introduced in (#144) #166
* [BUGFIX] Fix querier-frontend memcached arg #170

## 0.5.0 / 2021-06-08

* [CHANGE] **Labels has been updated to latest best practices**. This means that **you cannot do a rolling update from an older version of this chart**. Either change the labels of your existing deployment by hand to make the transition as seamless as possible or uninstall the old release and install the new release with a short downtime. #144
* [CHANGE] Minimum Kubernetes Version of this Chart is now 1.19 #144
* [CHANGE] A new dedicated service is now available for the memberlist. Check `config.memberlist.join_members` in the default `values.yaml` for more information. #144
* [CHANGE] Ingress is now disabled by default. You can re-enable it by setting `ingress.enabled` to `true` #144
* [CHANGE] Configs is now disabled by default. You can re-enable it by setting `configs.enabled` to `true` #144
* [CHANGE] PodDisruptionBudget is now enabled by default for all microservices once you have more than 1 replicas #144
* [ENHANCEMENT] startupProbe has been added to all Pods and the default values for `livenessProbe` and `readinessProbe` has been adjusted #144
* [ENHANCEMENT] You can now disable the alertmanager and ruler deployment by setting `ruler.enabled` or `alertmanager.enabled` to `false` #144
* [ENHANCEMENT] Rules for the ruler can now be provisioned via values. Check `ruler.directories` in the default `values.yaml` for more information. #144
* [ENHANCEMENT] Expose `client_max_body_size` config for nginx max request body size #137
* [ENHANCEMENT] Adding option to add custom headers (ex. X-Scope-OrgID) to NGINX from values.yaml (key `nginx.config.setHeaders`). #127
* [ENHANCEMENT] Headless service for distributor to allow GRPC load balancing #148
* [BUGFIX] Fixed `priorityClassName` in alertmanager deployment configuration. #155
* [BUGFIX] Replacing deprecated (in Cortex 1.9) configuration with appropriate replacements (max_look_back_period -> max_query_lookback & compress_responses -> response_compression_enabled). #154
* [BUGFIX] Added missing ServiceMonitors `additionalLabels` placeholders in `values.yaml`. #146

## 0.4.1 / 2021-03-22

* [BUGFIX] Fix dependencies in charts dir to reflect recent update in 0.4.0 #139

## 0.4.0 / 2021-03-01

* [CHANGE] Resource requests and limits have been removed from the values.yaml file. The default resources assigned will be based on the particular k8s cluster configuration, if they are not overridden. #112
* [CHANGE] Change memcached chart references from deprecated stable repo to bitnami source. #124
* [BUGFIX] Corrected structural issues found in ingress resource yaml for kube versions 1.19+. #112

## 0.3.0 / 2021-01-21

* [ENHANCEMENT] Expose new HTTP API paths for remote-write and Query frontend to nginx #103
* [ENHANCEMENT] Compactor service is no longer created if compactor is disabled. #82
* [ENHANCEMENT] Headless service for alert manager is only enabled when the alert manager is deployed as a stateful set. #91
* [ENHANCEMENT] Improved memcached configuration template, with support for both chunks & block storage caches. #92
* [ENHANCEMENT] Update apiVersion: networking.k8s.io/v1beta1 -> networking.k8s.io/v1 for Kubernetes versions 1.19+  #94
* [ENHANCEMENT] Service Monitors and Compactor Service #68, #80
* [ENHANCEMENT] Updated default replication factor to match documentation #102
* [BUGFIX] Update repository URL for memcache dependency charts. #96
* [BUGFIX] nginx config using non-default clusterDomain #78
* [BUGFIX] Add cluster DNS domain value #63
* [BUGFIX] Fix grpc port for querier #71

## 0.2.0 / 2020-10-25

* [FEATURE] Add support for Blocks storage backend of Cortex. #52
* [ENHANCEMENT] Adding dnsResolver option to nginx.config. #28
* [ENHANCEMENT] Supporting externally managed Cortex Config. #31
* [ENHANCEMENT] Enabling NGINX deployment to be optional. #32
* [ENHANCEMENT] Adding option to deploy ingester as StatefulSet. #46
* [ENHANCEMENT] Adding ability to run alertmanager as StatefulSet. #54
* [ENHANCEMENT] Updating README.MD with values.yaml defaults, adding alertmanager paths to nginx. #57
* [BUGFIX] Updating query frontend headless service to publish its address when not ready, to fix issue with queriers not becoming ready due to not finding any frontend addresses. #34
* [BUGFIX] Fixed invalid config template. #40
* [BUGFIX] Correct & add consistent target labels on components. #48
* [BUGFIX] Fix values file duplicate config.storage section of Cortex config. #51
* [BUGFIX] Add GRPC ports to store gateway pods and service. #52
* [BUGFIX] Fix service ports, fix ruler endpoint for configs and alertmanager. #55
