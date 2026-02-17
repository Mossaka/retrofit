---
on:
  workflow_dispatch:

engine: copilot
strict: false

network:
  allowed:
    - defaults
    - java
    - github
    - downloads.gradle-dn.com
    - scans.gradle.com
    - ge.solutions-team.gradle.com
    - central.sonatype.com
    - s01.oss.sonatype.org
    - oss.sonatype.org

safe-outputs:
  create-issue:
    max: 1
---

# Build and Test

## CRITICAL: Proxy Configuration for Java Builds

This environment uses a network proxy for all outbound connections. Before running ANY build commands, you MUST configure the JVM proxy settings. Run these commands FIRST:

```bash
# Configure JVM proxy for Gradle (reads SQUID_PROXY_HOST and SQUID_PROXY_PORT env vars)
export JAVA_TOOL_OPTIONS="-Dhttp.proxyHost=$SQUID_PROXY_HOST -Dhttp.proxyPort=$SQUID_PROXY_PORT -Dhttps.proxyHost=$SQUID_PROXY_HOST -Dhttps.proxyPort=$SQUID_PROXY_PORT -Dhttp.nonProxyHosts=localhost|127.0.0.1|host.docker.internal"

# Also configure Maven proxy in case any subproject uses Maven
mkdir -p ~/.m2
cat > ~/.m2/settings.xml << 'SETTINGS_EOF'
<settings>
  <proxies>
    <proxy>
      <id>awf-http</id><active>true</active><protocol>http</protocol>
      <host>${env.SQUID_PROXY_HOST}</host><port>${env.SQUID_PROXY_PORT}</port>
      <nonProxyHosts>localhost|127.0.0.1|host.docker.internal</nonProxyHosts>
    </proxy>
    <proxy>
      <id>awf-https</id><active>true</active><protocol>https</protocol>
      <host>${env.SQUID_PROXY_HOST}</host><port>${env.SQUID_PROXY_PORT}</port>
      <nonProxyHosts>localhost|127.0.0.1|host.docker.internal</nonProxyHosts>
    </proxy>
  </proxies>
</settings>
SETTINGS_EOF
```

You MUST run the above commands before attempting any build. Without them, dependency downloads will fail with "Network is unreachable" errors.

## Task

Explore this repository to understand its structure and purpose. Then build the project and run all tests. Finally, create an issue titled "Build and Test Report" with a detailed summary of:
- What the project is
- Build results (success/failure, any errors)
- Test results (number of tests passed/failed, any error details)
- Overall assessment of whether the build and tests succeeded
