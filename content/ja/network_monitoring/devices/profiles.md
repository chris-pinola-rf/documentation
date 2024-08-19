---
aliases:
- /ja/network_performance_monitoring/devices/profiles/
further_reading:
- link: /network_monitoring/devices/data
  tag: Documentation
  text: ネットワークデバイスモニタリングで収集されるデータ
- link: https://www.datadoghq.com/blog/monitor-snmp-with-datadog/
  tag: ブログ
  text: Datadog での SNMP モニタリング
title: NDM プロファイル
---

## Overview

Network Device Monitoring は、収集するメトリクスと関連タグを Datadog Agent に通知するためにプロファイルを使用します。プロファイルは、デバイスに関連付けられた OID のコレクションです。

## Configuration

By default, all profiles in the Agent configuration directory are loaded. To customize the specific profiles for collection, explicitly reference them by filename under `definition_file`, or provide an inline list under `definition`. Any of the Datadog profiles can be listed by name. Additional custom profiles can be referenced by the file path in the config, or placed in the configuration directory.

**注**: 一般的なプロファイルは [generic-device.yaml][1] で、ルーターやスイッチなどのデバイスをサポートしています。

<div class="alert alert-info">
GUI を使用してデバイスプロファイルを作成する場合は、<a href="/network_monitoring/devices/guide/device_profiles/">デバイスプロファイルの概要</a>ドキュメントを参照してください。
</div>

### sysOID mapped devices

Profiles allow Network Device Monitoring to reuse metric definitions across several device types or instances. Profiles define which metrics to collect and how to transform them into Datadog metrics. Each profile is expected to monitor a class of similar devices from the same vendor. They are automatically used by the Datadog Agent by comparing the sysObjectIds of the network device with the ones defined in the profile file.

The Datadog Agent provides out-of-the-box profiles in the `conf.d/snmp.d/default_profiles` directory. This directory is cleaned and reset upon Agent upgrades so do not save anything there. You can write your own custom profiles and extend existing ones by putting files in the `conf.d/snmp.d/profiles` directory.

The following example profile is used on any network device whose `sysobjectid` either _is_ `1.3.6.1.4.1.232.9.4.10` or _starts with_ `1.3.6.1.4.1.232.9.4.2.`:

```yaml
sysobjectid:
 - 1.3.6.1.4.1.232.9.4.10
 - 1.3.6.1.4.1.232.9.4.2.*

metrics:
  - MIB: CPQHLTH-MIB
    symbol:
      OID: 1.3.6.1.4.1.232.6.2.8.1.0
      name: cpqHeSysUtilLifeTime
```

If you need different metrics for network devices that share the same `sysobjectid`, you can write profiles without any `sysobjectid`, and configure the `profile` option in the SNMP configuration.

```yaml
instances:
   - ip_address: 192.168.34.10
     profile: my-profile1
   - ip_address: 192.168.34.11
     profile: my-profile2
   - ip_address: 192.168.34.13
     # For this device, the Agent will fetch the sysObjectID of the device and use the closest match
```

### Metric definition by profile

Profiles can be used interchangeably, meaning devices that share MIB dependencies can reuse the same profiles. For example, the [Cisco c3850 profile][2] can be used across many Cisco switches.

For more Datadog provided profiles, see the [GitHub repository][3].

### Metadata definition by profile

In the profiles's metadata section, you can define where and how metadata is collected. Values can be static or come from an OID value.
See the [DeviceMetadata][4] section for the supported fields.

With Datadog Agent version 7.52 and later, there is a `device_type` field for device metadata. This can be set manually in the profile and can be used to filter on specific types of devices. Accepted values include:

- access_point
- firewall
- load_balancer
- pdu
- printer
- router
- sd-wan
- sensor
- server
- storage
- switch
- ups
- wlc

See the [Profile Format Reference][5] for more information about profiles formats.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}


[1]: https://github.com/DataDog/integrations-core/blob/master/snmp/datadog_checks/snmp/data/default_profiles/generic-device.yaml
[2]: https://github.com/DataDog/integrations-core/blob/master/snmp/datadog_checks/snmp/data/default_profiles/cisco-3850.yaml
[3]: https://github.com/DataDog/integrations-core/tree/master/snmp/datadog_checks/snmp/data/default_profiles
[4]: https://github.com/DataDog/datadog-agent/blob/main/pkg/networkdevice/metadata/payload.go#L51-L76
[5]: https://datadoghq.dev/integrations-core/tutorials/snmp/profile-format/
[6]: https://app.datadoghq.com/devices/
[7]: /ja/network_monitoring/devices/guide/device_profiles/