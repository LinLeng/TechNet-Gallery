Markus Scholtes, 2022/01/04

There is only one possibility to export and import firewall rules: as a blob
(wfw file) in the firewall console or with a script. If you want to automate
removing or editing a rule from the set there is no (easy) way to do it without
using a third party tool or messing with the registry in dangerous places.

The three scripts ExportFirewallRules.ps1, ImportFirewallRules.ps1 and
RemoveFirewallRules.ps1 export, import and remove complete firewall rule sets in
CSV or JSON file format. When importing existing rules with the same display
name will be overwritten.

Requires Windows 8.1 / Server 2012 R2 or above.



Export-FirewallRules.ps1 [[-Name] <Object>] [[-CSVFile] <Object>] [-JSON] [-PolicyStore <String>] [-Inbound] [-Outbound] [-Enabled] [-Disabled] [-Allow] [-Block]

Exports firewall rules to a CSV or JSON file.

-Name
Displayname of the rules to be processed. Wildcard character * is allowed. Default: *
-CSVFile
Output file. Default: .\Firewall.csv
-JSON
Output in JSON instead of CSV format. Default: $FALSE
-PolicyStore
Store from which the rules are retrieved (default: ActiveStore).
Allowed values are PersistentStore, ActiveStore (the resultant rule set of all sources), localhost, a computer name, <domain.fqdn.com>\<GPO_Friendly_Name>, RSOP and others depending on the environment.
-Inbound -Outbound -Enabled -Disabled -Allow -Block
Filter which rules to export


Import-FirewallRules.ps1 [[-CSVFile] <Object>] [-JSON] [-PolicyStore <String>]

Imports firewall rules from a CSV or JSON file.

-CSVFile
Input file. Default: .\Firewall.csv
-JSON
Input in JSON instead of CSV format. Default: $FALSE
-PolicyStore
Store to which the rules are written (default: PersistentStore).
Allowed values are PersistentStore, ActiveStore (the resultant rule set of all sources), localhost, a computer name, <domain.fqdn.com>\<GPO_Friendly_Name> and others depending on the environment.



Remove-FirewallRules.ps1 [[-CSVFile] <Object>] [-JSON] [-PolicyStore <String>]

Remove firewall rules according to the list in a CSV or JSON file.

-CSVFile
Input file. Default: .\Firewall.csv
-JSON
Input in JSON instead of CSV format. Default: $FALSE
-PolicyStore
Store from which rules are removed (default: PersistentStore).
Allowed values are PersistentStore, ActiveStore (the resultant rule set of all sources), localhost, a computer name, <domain.fqdn.com>\<GPO_Friendly_Name> and others depending on the environment.



Examples (assume the scripts are in the current directory, modify path to scripts if not)

Export all firewall rules to the CSV file FirewallRules.csv in the current directory:

.\Export-FirewallRules.ps1


Export all SNMP firewall rules to the JSON file SNMPRules.json:

.\Export-FirewallRules.ps1 snmp* SNMPRules.json -json



Imports all firewall rules in the CSV file FirewallRules.csv in the current directory:

.\Import-FirewallRules.ps1


Imports all firewall rules in the JSON file WmiRules.json:

.\Import-FirewallRules.ps1 WmiRules.json -json



Remarks:

There might be issues when importing rules for "metro apps" to another computer.
App packet rules are stored as an SID and usually apply only to user accounts
whose SIDs are stored in the export file. Those rules will normally not work on
another computer since a SID is unique.
