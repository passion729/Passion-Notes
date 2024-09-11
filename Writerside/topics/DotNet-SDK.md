# DotNet SDK

- `wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh`
- `chmod +x ./dotnet-install.sh`
- `./dotnet-install.sh --version latest`

To install .NET Runtime instead of the SDK, use the --runtime parameter.

- `./dotnet-install.sh --version latest --runtime aspnetcore`

You can install a specific major version with the --channel parameter to indicate the specific version.

- `./dotnet-install.sh --channel 8.0`

```bash
mkdir -p $HOME/dotnet && tar zxf dotnet-sdk-7.0.408-linux-x64.tar.gz -C $HOME/dotnet
export DOTNET_ROOT=$HOME/dotnet
export PATH=$PATH:$DOTNET_ROOT:$DOTNET_ROOT/tools
```

### Reference
[Reference](https://learn.microsoft.com/en-us/dotnet/core/install/linux-scripted-manual#set-environment-variables-system-wide)

