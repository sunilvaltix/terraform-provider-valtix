# Resource: valtix_profile_network_intrusion

Create IPS (Network Intrusion) Profile

## Example Usage

```hcl
resource "valtix_profile_network_intrusion" ips1 {
  name                  = "ips"
  description           = "predefined rules tagged as 'connectivity'"
  talos_ruleset_version = "2.9.11-05102020"
  policy                = "CONNECTIVITY"
  policy_action         = "ALERT"
}
```

## Argument Reference

* `name` - (Required) Name of the IPS profile
* `description` - (Optional) Description of the IPS profile
* `talos_ruleset_version` - (Required) IPS ruleset (from Talos) version. Valid values:
* `action` - (Optional) Default action for all the attacks. Can be overwritten for each attack type. Default value is specified in the talos rule set. If this is not specified then whatever action that's specified in the ruleset is used (called Rule Default). Valid values:
    * **ALERT** (logs in ips events)
    * **DROP** (drop the traffic and log the events)
    * **PASS** (pass the traffic and no log of events)
    * **REJECT** (send a reset)
* `policy` - (Required) Pre-defined IPS ruleset to use, must be one of:
    * **CONNECTIVITY**
    * **BALANCED**,
    * **SECURITY**
    * **MAX_DETECT**
* `policy_action` - (Optional) Action to apply for the predefined policy. Look at action attribute for valid values. If this is not specified, the action defined in the 'action' attribute is used.
* `categories` - (Optional) List of predefined categories. Structure [defined below](#categories)
* `classes` - (Optional) List of predefined classes. Structure [defined below](#classes)
* `pcap` - (Optional) true/false. Capture pcap when traffic matches the IPS rules
* `rule_event_filter` - (Optional) Rate Limit / Sample a set of rules. Structure is [defined below](#rule-event-filter)
* `event_suppressor` - (Optional) Suppress a given set of rule ids for traffic from certain sources. Structure is [defined below](#event-suppressor)
* `profile_event_filter` - (Optional) Similar to the rule_event_filter but applies to the whole profile instead of specific rule(s).  Structure is [defined below](#profile-event-filter)

## Categories
* `name` - (Required) Name of IPS attacks categories
* `action` - (Optional) Values same as action attribute. If this is not specified, then the value defined in 'action' attribute is used

## Classes

* `name` - (Required) Name of IPS attacks classes
* `action` - (Optional) Values same as action attribute. If this is not specified, then the value defined in 'action' attribute is used

## Rule Event Filter

* `rule_ids` - (Optional) List of rule ids to filter
* `type` - (Optional) "RATE" or "SAMPLE". When "RATE" is selected, number_of_events and time must be provided. action is applied once the provided rule_ids match the given count in the given time.

If the type is "SAMPLE", the action is applied once the count of the events matces

* `number_of_events` - (Optional) Number of times the rule id attack must match before the action is applied
* `time` - (Optional) Used when the type is "RATE", the number_of_events must match in the given time (in seconds)

## Event Suppressor

* `source_ips` - (Optional) List of source ips or CIDRs
* `rule_ids` - (Optional) List of rule ids to filter

## Profile Event Filter

* `rule_ids` - (Optional) List of rule ids to filter
* `type` - (Optional) "RATE" or "SAMPLE". When "RATE" is selected, number_of_events and time must be provided. action is applied once the provided rule_ids match the given count in the given time.

If the type is "SAMPLE", the action is applied once the count of the events matces