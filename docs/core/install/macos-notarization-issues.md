---
title: 处理 macOS Catalina 公证
description: 本文介绍在安装 .NET core 运行时、SDK 以及使用 .NET Core 生成的应用时，如何处理 macOS 的公证和证书问题。
author: thraka
ms.author: adegeo
ms.date: 02/14/2020
ms.openlocfilehash: be39c1ea56699f84736a2b37bc958507b16e826b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146744"
---
# <a name="macos-catalina-notarization-and-the-impact-on-net-core-downloads-and-projects"></a><span data-ttu-id="5a518-103">macOS Catalina 公证以及对 .NET Core 下载和项目的影响</span><span class="sxs-lookup"><span data-stu-id="5a518-103">macOS Catalina Notarization and the impact on .NET Core downloads and projects</span></span>

<span data-ttu-id="5a518-104">自 macOS Catalina（版本 10.15）开始，所有在 2019 年 6 月 1 日之后生成并使用开发者 ID 扩散的软件都必须经过公证。</span><span class="sxs-lookup"><span data-stu-id="5a518-104">Beginning with macOS Catalina (version 10.15), all software built after June 1, 2019, and distributed with Developer ID, must be notarized.</span></span> <span data-ttu-id="5a518-105">此要求应用于 .NET Core 运行时、.NET Core SDK 以及使用 .NET Core 创建的软件。</span><span class="sxs-lookup"><span data-stu-id="5a518-105">This requirement applies to the .NET Core runtime, .NET Core SDK, and software created with .NET Core.</span></span> <span data-ttu-id="5a518-106">本文介绍了 .NET Core 和 macOS 公证可能会遇到的常见情况。</span><span class="sxs-lookup"><span data-stu-id="5a518-106">This article describes the common scenarios you may face with .NET Core and macOS notarization.</span></span>

## <a name="installing-net-core"></a><span data-ttu-id="5a518-107">安装 .NET Core</span><span class="sxs-lookup"><span data-stu-id="5a518-107">Installing .NET Core</span></span>

<span data-ttu-id="5a518-108">自 2020 年 2 月 18 日起，.NET Core（运行时和 SDK）版本 3.1、3.0 和 2.1 的安装程序都已经过公证。</span><span class="sxs-lookup"><span data-stu-id="5a518-108">The installers for .NET Core (both runtime and SDK) versions 3.1, 3.0, and 2.1, have been notarized since February 18, 2020.</span></span> <span data-ttu-id="5a518-109">以前发布的版本没有经过公证。</span><span class="sxs-lookup"><span data-stu-id="5a518-109">Prior released versions aren't notarized.</span></span> <span data-ttu-id="5a518-110">通过先下载安装程序，然后使用 `sudo installer` 命令，可以手动安装 .NET Core 的未公证版本。</span><span class="sxs-lookup"><span data-stu-id="5a518-110">You can manually install a non-notarized version of .NET Core by first downloading the installer, and then using the `sudo installer` command.</span></span> <span data-ttu-id="5a518-111">有关详细信息，请参阅[下载并手动安装 macOS](sdk.md?pivots=os-macos#download-and-manually-install)。</span><span class="sxs-lookup"><span data-stu-id="5a518-111">For more information, see [Download and manually install for macOS](sdk.md?pivots=os-macos#download-and-manually-install).</span></span>

<span data-ttu-id="5a518-112">自以下版本开始，.NET Core 安装程序未经过公证：</span><span class="sxs-lookup"><span data-stu-id="5a518-112">Beginning with the following versions, the .NET Core installers are notarized:</span></span>

- <span data-ttu-id="5a518-113">.NET Core 运行时</span><span class="sxs-lookup"><span data-stu-id="5a518-113">.NET Core Runtime</span></span>
  - <span data-ttu-id="5a518-114">2.1.16</span><span class="sxs-lookup"><span data-stu-id="5a518-114">2.1.16</span></span>
  - <span data-ttu-id="5a518-115">3.0.3</span><span class="sxs-lookup"><span data-stu-id="5a518-115">3.0.3</span></span>
  - <span data-ttu-id="5a518-116">3.1.2</span><span class="sxs-lookup"><span data-stu-id="5a518-116">3.1.2</span></span>

- <span data-ttu-id="5a518-117">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="5a518-117">.NET Core SDK</span></span>
  - <span data-ttu-id="5a518-118">2.1.512</span><span class="sxs-lookup"><span data-stu-id="5a518-118">2.1.512</span></span>
  - <span data-ttu-id="5a518-119">3.0.103</span><span class="sxs-lookup"><span data-stu-id="5a518-119">3.0.103</span></span>
  - <span data-ttu-id="5a518-120">3.1.102</span><span class="sxs-lookup"><span data-stu-id="5a518-120">3.1.102</span></span>

## <a name="apphost-is-disabled-by-default"></a><span data-ttu-id="5a518-121">默认禁用 appHost</span><span class="sxs-lookup"><span data-stu-id="5a518-121">appHost is disabled by default</span></span>

<span data-ttu-id="5a518-122">默认情况下，当项目编译、发布或运行时，.NET Core SDK 3.0 及更高版本的未公证版本会生成一个本机 Mach-O 可执行文件（即 appHost）  。</span><span class="sxs-lookup"><span data-stu-id="5a518-122">By default, non-notarized versions of the .NET Core SDK 3.0 and above produce a native Mach-O executable (known as the **appHost**) when your project compiles, publishes, or is run.</span></span> <span data-ttu-id="5a518-123">此可执行文件是运行应用的一种简便方法。</span><span class="sxs-lookup"><span data-stu-id="5a518-123">This executable is a convenient way to run your app.</span></span> <span data-ttu-id="5a518-124">否则必须通过运行 `dotnet <filename.dll>` 启动应用。</span><span class="sxs-lookup"><span data-stu-id="5a518-124">Otherwise, your app must be started by running `dotnet <filename.dll>`.</span></span> <span data-ttu-id="5a518-125">启用 appHost 后，会在 appHost 的上下文中调用 `dotnet run` 命令。</span><span class="sxs-lookup"><span data-stu-id="5a518-125">When the appHost is enabled, the `dotnet run` command is invoked in the context of the appHost.</span></span> <span data-ttu-id="5a518-126">有关详细信息，请参阅 [appHost 的上下文](#context-of-the-apphost)。</span><span class="sxs-lookup"><span data-stu-id="5a518-126">For more information, see [Context of the appHost](#context-of-the-apphost).</span></span>

<span data-ttu-id="5a518-127">从 .NET Core SDK 3.0 及更高版本的已公证版本开始，默认情况下不生成 appHost 可执行文件。</span><span class="sxs-lookup"><span data-stu-id="5a518-127">Starting with the notarized versions of the .NET Core SDK 3.0 and above, the appHost executable isn't generated by default.</span></span> <span data-ttu-id="5a518-128">可以通过项目文件中的 [`UseAppHost`](../project-sdk/msbuild-props.md#useapphost) 布尔值设置来启用 appHost 生成。</span><span class="sxs-lookup"><span data-stu-id="5a518-128">You can turn on appHost generation with the [`UseAppHost`](../project-sdk/msbuild-props.md#useapphost) boolean setting in the project file.</span></span> <span data-ttu-id="5a518-129">还可以在运行的特定 `dotnet` 命令的命令行上通过 `-p:UseAppHost` 参数切换 appHost 的启用状态：</span><span class="sxs-lookup"><span data-stu-id="5a518-129">You can also toggle the appHost with the `-p:UseAppHost` parameter on the command line for the specific `dotnet` command you run:</span></span>

- <span data-ttu-id="5a518-130">项目文件</span><span class="sxs-lookup"><span data-stu-id="5a518-130">Project file</span></span>

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- <span data-ttu-id="5a518-131">命令行参数</span><span class="sxs-lookup"><span data-stu-id="5a518-131">Command-line parameter</span></span>

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

<span data-ttu-id="5a518-132">发布[独立](../deploying/index.md#publish-self-contained)应用时，始终会创建 appHost。</span><span class="sxs-lookup"><span data-stu-id="5a518-132">An appHost is always created when you publish your app [self-contained](../deploying/index.md#publish-self-contained).</span></span>

<span data-ttu-id="5a518-133">有关 `UseAppHost` 设置的详细信息，请参阅 [Microsoft.NET.Sdk 的 MSBuild 属性](../project-sdk/msbuild-props.md#useapphost)。</span><span class="sxs-lookup"><span data-stu-id="5a518-133">For more information about the `UseAppHost` setting, see [MSBuild properties for Microsoft.NET.Sdk](../project-sdk/msbuild-props.md#useapphost).</span></span>

### <a name="context-of-the-apphost"></a><span data-ttu-id="5a518-134">AppHost 的上下文</span><span class="sxs-lookup"><span data-stu-id="5a518-134">Context of the appHost</span></span>

<span data-ttu-id="5a518-135">在项目中启用了 appHost 并使用 `dotnet run` 命令运行应用时，将在 appHost 的上下文中调用应用，而不是在默认主机（默认主机对应的是 `dotnet` 命令）的上下文中调用。</span><span class="sxs-lookup"><span data-stu-id="5a518-135">When the appHost is enabled in your project, and you use the `dotnet run` command to run your app, the app is invoked in the context of the appHost and not the default host (the default host is the `dotnet` command).</span></span> <span data-ttu-id="5a518-136">如果在项目中禁用了 appHost，则 `dotnet run` 命令将在默认主机的上下文中运行应用。</span><span class="sxs-lookup"><span data-stu-id="5a518-136">If the appHost is disabled in your project, the `dotnet run` command runs your app in the context of the default host.</span></span> <span data-ttu-id="5a518-137">即使禁用了 appHost，如果发布独立应用，也会生成一份 appHost 可执行文件，且用户也可以使用该可执行文件运行应用。</span><span class="sxs-lookup"><span data-stu-id="5a518-137">Even if the appHost is disabled, publishing your app as self-contained generates an appHost executable, and users use that executable to run your app.</span></span> <span data-ttu-id="5a518-138">使用 `dotnet <filename.dll>` 运行应用会通过默认主机（共享运行时）调用应用。</span><span class="sxs-lookup"><span data-stu-id="5a518-138">Running your app with `dotnet <filename.dll>` invokes the app with the default host, the shared runtime.</span></span>

<span data-ttu-id="5a518-139">调用使用 appHost 的应用时，应用访问的证书分区与已公证的默认主机不同。</span><span class="sxs-lookup"><span data-stu-id="5a518-139">When an app using the appHost is invoked, the certificate partition accessed by the app is different from the notarized default host.</span></span> <span data-ttu-id="5a518-140">如果应用必须访问通过默认主机安装的证书，请使用 `dotnet run` 命令从应用的项目文件运行它，或使用 `dotnet <filename.dll>` 命令直接启动该应用。</span><span class="sxs-lookup"><span data-stu-id="5a518-140">If your app must access the certificates installed through the default host, use the `dotnet run` command to run your app from its project file, or use the `dotnet <filename.dll>` command to start the app directly.</span></span>

<span data-ttu-id="5a518-141">有关此方案的详细信息，请参阅 [ASP.NET Core 和 macOS 和证书](#aspnet-core-and-macos-and-certificates)部分。</span><span class="sxs-lookup"><span data-stu-id="5a518-141">More information about this scenario is provided in the [ASP.NET Core and macOS and certificates](#aspnet-core-and-macos-and-certificates) section.</span></span>

## <a name="aspnet-core-and-macos-and-certificates"></a><span data-ttu-id="5a518-142">ASP.NET Core 和 macOS 和证书</span><span class="sxs-lookup"><span data-stu-id="5a518-142">ASP.NET Core and macOS and certificates</span></span>

<span data-ttu-id="5a518-143">使用 .NET Core，可以使用 <xref:System.Security.Cryptography.X509Certificates> 类在 macOS Keychain 中管理证书。</span><span class="sxs-lookup"><span data-stu-id="5a518-143">.NET Core provides the ability to manage certificates in the macOS Keychain with the <xref:System.Security.Cryptography.X509Certificates> class.</span></span> <span data-ttu-id="5a518-144">在决定要考虑哪个分区时，对 macOS Keychain 的访问将使用应用程序标识作为主键。</span><span class="sxs-lookup"><span data-stu-id="5a518-144">Access to the macOS Keychain uses the applications identity as the primary key when deciding which partition to consider.</span></span> <span data-ttu-id="5a518-145">例如，未签名的应用程序会将机密存储在未签名分区中，而已签名应用程序会将其机密存储在仅能由它们访问的分区中。</span><span class="sxs-lookup"><span data-stu-id="5a518-145">For example, unsigned applications store secrets in the unsigned partition, but signed applications store their secrets in partitions only they can access.</span></span> <span data-ttu-id="5a518-146">要使用的分区取决于调用应用的执行源。</span><span class="sxs-lookup"><span data-stu-id="5a518-146">The source of execution that invokes your app decides which partition to use.</span></span>

<span data-ttu-id="5a518-147">.NET Core 提供三个执行源：[appHost](#apphost-is-disabled-by-default)、默认主机（`dotnet` 命令）和自定义主机。</span><span class="sxs-lookup"><span data-stu-id="5a518-147">.NET Core provides three sources of execution: [appHost](#apphost-is-disabled-by-default), default host (the `dotnet` command), and a custom host.</span></span> <span data-ttu-id="5a518-148">每个执行模型都可能具有不同的标识（已签名或未签名），并可以访问 Keychain 中的不同分区。</span><span class="sxs-lookup"><span data-stu-id="5a518-148">Each execution model may have different identities, either signed or unsigned, and has access to different partitions within the Keychain.</span></span> <span data-ttu-id="5a518-149">通过一种模式导入的证书可能不能通过另一种模式访问。</span><span class="sxs-lookup"><span data-stu-id="5a518-149">Certificates imported by one mode may not be accessible from another.</span></span> <span data-ttu-id="5a518-150">例如，已公证版本的 .NET Core 具有已签名的默认主机。</span><span class="sxs-lookup"><span data-stu-id="5a518-150">For example, the notarized versions of .NET Core have a default host that is signed.</span></span> <span data-ttu-id="5a518-151">根据证书的标识将证书导入到安全分区中。</span><span class="sxs-lookup"><span data-stu-id="5a518-151">Certificates are imported into a secure partition based on its identity.</span></span> <span data-ttu-id="5a518-152">由于 appHost 是未签名的，因此无法从生成的 appHost 访问这些证书。</span><span class="sxs-lookup"><span data-stu-id="5a518-152">These certificates aren't accessible from a generated appHost, as the appHost is unsigned.</span></span>

<span data-ttu-id="5a518-153">再举一例，默认情况下，ASP.NET Core 通过默认主机导入默认 SSL 证书。</span><span class="sxs-lookup"><span data-stu-id="5a518-153">Another example, by default, ASP.NET Core imports a default SSL certificate through the default host.</span></span> <span data-ttu-id="5a518-154">使用 appHost 的 ASP.NET Core 应用程序将不能访问此证书，并且当 .NET Core 检测到无法访问该证书时，将收到错误消息。</span><span class="sxs-lookup"><span data-stu-id="5a518-154">ASP.NET Core applications that use an appHost won't have access to this certificate and will receive an error when .NET Core detects the certificate isn't accessible.</span></span> <span data-ttu-id="5a518-155">该错误消息中会提供关于如何解决此问题的说明。</span><span class="sxs-lookup"><span data-stu-id="5a518-155">The error message provides instructions on how to fix this problem.</span></span>

<span data-ttu-id="5a518-156">如果证书共享是必需的，macOS 会提供带有 `security` 实用工具的配置选项。</span><span class="sxs-lookup"><span data-stu-id="5a518-156">If certificate sharing is required, macOS provides configuration options with the `security` utility.</span></span>

<span data-ttu-id="5a518-157">如需详细了解如何解决 ASP.NET Core 证书问题，请参阅[在 ASP.NET Core 中强制执行 HTTPS](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1&tabs=visual-studio#troubleshoot-certificate-problems)。</span><span class="sxs-lookup"><span data-stu-id="5a518-157">For more information on how to troubleshoot ASP.NET Core certificate issues, see [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1&tabs=visual-studio#troubleshoot-certificate-problems).</span></span>

## <a name="default-entitlements"></a><span data-ttu-id="5a518-158">默认权利</span><span class="sxs-lookup"><span data-stu-id="5a518-158">Default entitlements</span></span>

<span data-ttu-id="5a518-159">.NET Core 的默认主机（`dotnet` 命令）具有一套默认权利。</span><span class="sxs-lookup"><span data-stu-id="5a518-159">.NET Core’s default host (the `dotnet` command) has a set of default entitlements.</span></span> <span data-ttu-id="5a518-160">需要这些权利才能正常运行 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="5a518-160">These entitlements are required for proper operation of .NET Core.</span></span> <span data-ttu-id="5a518-161">你的应用程序可能需要其他权利，在这种情况下，需要生成并使用 [appHost](#apphost-is-disabled-by-default)，然后在本地添加必要的权利。</span><span class="sxs-lookup"><span data-stu-id="5a518-161">It's possible that your application may need additional entitlements, in which case you'll need to generate and use an [appHost](#apphost-is-disabled-by-default) and then add the necessary entitlements locally.</span></span>

<span data-ttu-id="5a518-162">.NET Core 的默认权利包括：</span><span class="sxs-lookup"><span data-stu-id="5a518-162">Default set of entitlements for .NET Core:</span></span>

- `com.apple.security.cs.allow-jit`
- `com.apple.security.cs.allow-unsigned-executable-memory`
- `com.apple.security.cs.allow-dyld-environment-variables`
- `com.apple.security.cs.disable-library-validation`

## <a name="notarize-a-net-core-app"></a><span data-ttu-id="5a518-163">对 .NET Core 应用进行公证</span><span class="sxs-lookup"><span data-stu-id="5a518-163">Notarize a .NET Core app</span></span>

<span data-ttu-id="5a518-164">如果希望应用程序在 macOS Catalina（版本 10.15）或更高版本上运行，则需要对应用进行公证。</span><span class="sxs-lookup"><span data-stu-id="5a518-164">If you want your application to run on macOS Catalina (version 10.15) or higher, you'll want to notarize your app.</span></span> <span data-ttu-id="5a518-165">为了进行公证而随应用程序提交的 appHost 应至少具备与 .NET Core 相同的[默认权利](#default-entitlements)。</span><span class="sxs-lookup"><span data-stu-id="5a518-165">The appHost you submit with your application for notarization should be used with at least the same [default entitlements](#default-entitlements) for .NET Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a518-166">后续步骤</span><span class="sxs-lookup"><span data-stu-id="5a518-166">Next steps</span></span>

- <span data-ttu-id="5a518-167">[.NET Core 依赖项和要求](dependencies.md)。</span><span class="sxs-lookup"><span data-stu-id="5a518-167">[.NET Core dependencies and requirements](dependencies.md).</span></span>
- <span data-ttu-id="5a518-168">[安装 .NET Core SDK](sdk.md)。</span><span class="sxs-lookup"><span data-stu-id="5a518-168">[Install the .NET Core SDK](sdk.md).</span></span>
- [<span data-ttu-id="5a518-169">安装 .NET Core 运行时</span><span class="sxs-lookup"><span data-stu-id="5a518-169">Install the .NET Core Runtime</span></span>](runtime.md)