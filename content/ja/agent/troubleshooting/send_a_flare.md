---
algolia:
  tags:
  - Agent フレア
aliases:
- /ja/agent/faq/send-logs-and-configs-to-datadog-via-flare-command
further_reading:
- link: /agent/troubleshooting/debug_mode/
  tag: Documentation
  text: Agent デバッグモード
- link: /agent/troubleshooting/agent_check_status/
  tag: Documentation
  text: Agent チェックのステータスを確認
title: Agent フレア
---

{{< site-region region="gov" >}}
<div class="alert alert-warning">Sending an Agent Flare is not supported for this site.</div>
{{< /site-region >}}

フレアを使用すると、必要なトラブルシューティング情報を Datadog のサポートチームに送信できます。

このページでは、以下の内容を説明しています。
- [`flare` コマンドを使用したフレアの送信](#send-a-flare-using-the-flare-command)。
- Remote Configuration を使用した [Datadog サイトからのフレアの送信](#send-a-flare-from-the-datadog-site)。
- [手動送信](#manual-submission)。

A flare gathers all of the Agent's configuration files and logs into an archive file. It removes sensitive information, including passwords, API keys, Proxy credentials, and SNMP community strings.

The Datadog Agent is completely open source, which allows you to [verify the code's behavior][1]. If needed, the flare can be reviewed prior to sending since the flare prompts a confirmation before uploading it.

## Datadog サイトからフレアを送信する

Datadog サイトからフレアを送信するには、Agent の [Fleet Automation][2] と [Remote configuration][3] が有効になっていることを確認してください。

{{% remote-flare %}}

{{< img src="agent/fleet_automation/fleet-automation-flares2.png" alt="The Send Ticket button launches a form to send a flare for an existing or new support ticket" style="width:70%;" >}}

## `flare` コマンドを使用してフレアを送信する

`flare` サブコマンドを使用してフレアを送信します。以下のコマンドで、`<CASE_ID>` を実際の Datadog サポートケース ID (ある場合) に置き換え、それに紐づけされているメールアドレスを入力します。

ケース ID がない場合は、Datadog へのログインに使用するメールアドレスを入力して新しいサポートケースを作成します。

**アーカイブのアップロードを確認し、直ちに Datadog サポートに送信してください**。

{{< tabs >}}
{{% tab "Agent" %}}

| Platform   | Command                                                 |
|------------|---------------------------------------------------------|
| AIX        | `datadog-agent flare <CASE_ID>`                         |
| Docker     | `docker exec -it dd-agent agent flare <CASE_ID>`        |
| macOS      | `datadog-agent flare <CASE_ID>` or via the [web GUI][1] |
| CentOS     | `sudo datadog-agent flare <CASE_ID>`                    |
| Debian     | `sudo datadog-agent flare <CASE_ID>`                    |
| Kubernetes | `kubectl exec -it <AGENT_POD_NAME> -- agent flare <CASE_ID>`  |
| Fedora     | `sudo datadog-agent flare <CASE_ID>`                    |
| Redhat     | `sudo datadog-agent flare <CASE_ID>`                    |
| Suse       | `sudo datadog-agent flare <CASE_ID>`                    |
| Source     | `sudo datadog-agent flare <CASE_ID>`                    |
| Windows    | Consult the dedicated [Windows documentation][2]        |
| Heroku     | Consult the dedicated [Heroku documentation][3]         |
| PCF     | `sudo /var/vcap/jobs/dd-agent/packages/dd-agent/bin/agent/agent flare <CASE_ID>`             |

## Dedicated containers

Agent v7.19 以降を使用し、Datadog Helm Chart を[最新バージョン][4]で使用するか、Datadog Agent と Trace Agent が別々のコンテナにある DaemonSet を使用する場合は、以下を含む Agent Pod をデプロイします。

* One container with the Agent process (Agent + Log Agent)
* One container with the process-agent process
* One container with the trace-agent process
* One container with the system-probe process

To get a flare from each container, run the following commands:

### Agent

```bash
kubectl exec -it <AGENT_POD_NAME> -c agent -- agent flare <CASE_ID>
```

### Process Agent

```bash
kubectl exec -it <AGENT_POD_NAME> -c process-agent -- agent flare <CASE_ID> --local
```

### Trace Agent

```bash
kubectl exec -it <AGENT_POD_NAME> -c trace-agent -- agent flare <CASE_ID> --local
```

### Security Agent

```bash
kubectl exec -it <AGENT_POD_NAME> -c security-agent -- security-agent flare <CASE_ID>
```

### System probe

The system-probe container cannot send a flare so get container logs instead:

```bash
kubectl logs <AGENT_POD_NAME> -c system-probe > system-probe.log
```

## ECS Fargate

ECS Fargate プラットフォーム v1.4.0 を使用する場合、[Amazon ECS Exec][5] を有効にすることで、実行中の Linux コンテナへのアクセスを許可するように ECS タスクとサービスを構成できます。Amazon ECS Exec を有効にした後、次のコマンドを実行してフレアを送信します。

```bash
aws ecs execute-command --cluster <CLUSTER_NAME> \
    --task <TASK_ID> \
    --container datadog-agent \
    --interactive \
    --command "agent flare <CASE_ID>"
```

**注:** ECS Exec は、新しいタスクに対してのみ有効にできます。ECS Exec を使用するには、既存のタスクを再作成する必要があります。

[1]: /ja/agent/basic_agent_usage/#gui
[2]: /ja/agent/basic_agent_usage/windows/#agent-v6
[3]: /ja/agent/guide/heroku-troubleshooting/#send-a-flare
[4]: https://github.com/DataDog/helm-charts/blob/master/charts/datadog/CHANGELOG.md
[5]: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-exec.html
{{% /tab %}}

{{% tab "Cluster Agent" %}}

| Platform      | Command                                                                     |
|---------------|-----------------------------------------------------------------------------|
| Kubernetes    | `kubectl exec -n <NAMESPACE> -it <CLUSTER_POD_NAME> -- datadog-cluster-agent flare <CASE_ID>` |
| Cloud Foundry | `/var/vcap/packages/datadog-cluster-agent/datadog-cluster-agent-cloudfoundry flare -c /var/vcap/jobs/datadog-cluster-agent/config <CASE_ID>` |

{{% /tab %}}
{{< /tabs >}}

## Manual submission

The Agent flare protocol collects configurations and logs into an archive file first located in the local `/tmp` directory.
Manually obtain this file and provide it to support if there are any issues with Agent connectivity.

### Kubernetes
To obtain the archive file in Kubernetes, use the kubectl command:
```
kubectl cp datadog-<pod-name>:tmp/datadog-agent-<date-of-the-flare>.zip flare.zip -c agent
```

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://github.com/DataDog/datadog-agent/tree/main/pkg/flare
[2]: /ja/agent/fleet_automation/
[3]: /ja/agent/remote_config#enabling-remote-configuration