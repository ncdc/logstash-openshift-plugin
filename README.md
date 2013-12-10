logstash-openshift-plugin
=========================

Plugin for getting OpenShift gear environment variables added as fields to events.

Example configuration
=====================

    input {
      file {
        path => ["/var/lib/openshift/**/log/*", "/var/lib/openshift/**/logs/*"]
        stat_interval => 1
        type => "gear_log"
      }
    }

    filter {
      if [type] == "gear_log" {
        grok {
          match => {
            "path" => ["/var/lib/openshift/(?<gear_uuid>[^/]+)/"]
          }
        }

        openshift {
        	variables => [
        	  "OPENSHIFT_APP_NAME",
        	  "OPENSHIFT_NAMESPACE"
          ]
        }
      }
    }

    output {
    	stdout { }
    }

Notes
=====
In order for this filter to function, there must already be a `gear_uuid` field present in the event. In the example above, this comes from the grok filter, which determines the gear_uuid based on the `path` field's value.

License
=======
Released under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
