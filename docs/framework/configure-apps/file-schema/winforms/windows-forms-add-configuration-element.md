---
title: Windows 窗体添加配置元素
ms.date: 04/07/2017
helpviewer_keywords:
- Windows Forms Add configuration element
- configuring Windows Forms applications
ms.assetid: 3e3e04de-99d1-4658-b716-44cb669d9589
ms.openlocfilehash: 26b806f84c3e1bc44e0437a8f8806316b14897b8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73109663"
---
# <a name="windows-forms-add-configuration-element"></a>Windows 窗体添加配置元素

`<add>` 元素将添加一个预定义的密钥，该密钥指定 Windows 窗体应用程序是否支持添加到 .NET Framework 4.7 或更高版本中 Windows 窗体应用程序的功能。

## <a name="syntax"></a>语法

```xml
<System.Windows.Forms.ApplicationConfigurationSection>
  <add key="key-name" value="key-value" />
</System.Windows.Forms.ApplicationConfigurationSection>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

| 特性 | 描述 |
| --------- | ----------- |
| `key`     | 必需的特性。 与特定 Windows 窗体自定义功能对应的预定义密钥名称。 |
| `value`   | 必需的特性。 要赋给 `key` 的值。 |

### <a name="key-attribute-names-and-associated-values"></a>`key` 属性名称和关联值

| `key` 名称 | 值 | 描述 |
| ---------- | ------ | ----------- |
| "AnchorLayout.DisableSinglePassControlScaling" | "true&#124;" | 指示是否在单个传递中缩放定位控件。 若要禁用单一传递缩放，则为 "true";否则为 false。 有关详细信息，请参阅 "[备注](#remarks)" 中的 "单一传递缩放" 部分。 |
| "DpiAwareness" | "PerMonitorV2"&#124;"false" | 指示应用程序是否可感知 DPI。 将 "密钥" 设置为 "PerMonitorV2" 以支持 Dpi 识别;否则，请将其设置为 "false"。 DPI 感知是一项选择功能;若要利用 Windows 窗体的高 DPI 支持，应将其值设置为 "PerMonitorV2"。 有关详细信息，请参阅 "[备注](#remarks)" 部分。 |
| "CheckedListBox. DisableHighDpiImprovements" | "true&#124;" | 指示 <xref:System.Windows.Forms.CheckedListBox> 控件是否利用 .NET Framework 4.7 中引入的缩放和布局改进功能。 如果选择不进行缩放和布局改进，则为 "true";否则为 "false"。 |
| "DisableHighDpiImprovements" | "true&#124;" | 指示 <xref:System.Windows.Forms.DataGridView> 控件是否在 .NET Framework 4.7 中引入了缩放和布局改进。 如果选择退出 DPI 感知，则为 "true";否则为 "false"。 |
| "DisableDpiChangedMessageHandling" | "true&#124;" | 如果选择不接收与 DPI 缩放更改相关的消息，则为 "true";否则为 "false"。 有关详细信息，请参阅 "[备注](#remarks)" 部分。 |
| EnableWindowsFormsHighDpiAutoResizing | "true&#124;" | 指示是否由于 DPI 缩放变化而自动调整 Windows 窗体应用程序的大小。 若要启用自动调整大小，则为 "true";否则为 false。 |
| "DisableSinglePassControlScaling" | "true&#124;" | 指示是否在单个传递中缩放 <xref:System.Windows.Forms.Form>。 若要禁用单步缩放，则为 "true";否则为 false。 有关详细信息，请参阅 "[备注](#remarks)" 中的 "单一传递缩放" 部分。 |
| "MonthCalendar. DisableSinglePassControlScaling" | "true&#124;" | 指示是否在单个传递中缩放 <xref:System.Windows.Forms.MonthCalendar> 控件。 若要禁用单步缩放，则为 "true";否则为 false。 有关详细信息，请参阅 "[备注](#remarks)" 中的 "单一传递缩放" 部分。 |
| "DisableHighDpiImprovements" | "true&#124;" | 指示 <xref:System.Windows.Forms.ToolStrip> 控件是否利用 .NET Framework 4.7 中引入的缩放和布局改进功能。 如果选择退出 DPI 感知，则为 "true";否则为 "false"。 |

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

| 元素 | 描述 |
| ------- | ----------- |
| [`<System.Windows.Forms.ApplicationConfigurationSection>`](index.md) | 配置对新 Windows 窗体应用程序功能的支持。 |

## <a name="remarks"></a>备注

从 .NET Framework 4.7 开始，`<System.Windows.Forms.ApplicationConfigurationSection>` 元素允许配置 Windows 窗体应用程序，以利用最新版本的 .NET Framework 中添加的功能。

`<System.Windows.Forms.ApplicationConfigurationSection>` 元素允许添加一个或多个子 `<add>` 元素，其中每个元素都定义了一个特定的配置设置。

有关 Windows 窗体高 DPI 支持的概述，请参阅[Windows 窗体中的高 Dpi 支持](../../../winforms/high-dpi-support-in-windows-forms.md)。

### <a name="dpiawareness"></a>DpiAwareness

在 Windows 版本下运行的 Windows 窗体应用（从 Windows 10 创意者版开始）以及从 .NET Framework 4.7 开始的目标版本 .NET Framework 可配置为利用 .NET Framework 中引入的高 DPI 改进4.7。 这些方法包括：

- 支持动态 DPI 方案，在此方案中，用户在启动 Windows 窗体应用程序后更改了 DPI 或缩放系数。

- 改进了许多 Windows 窗体控件（如 <xref:System.Windows.Forms.MonthCalendar> 控件和 <xref:System.Windows.Forms.CheckedListBox> 控件）的缩放和布局。

高 DPI 感知是一项选择功能;默认情况下，将 `false``DpiAwareness` 的值。 你可以通过在应用程序配置文件中将此密钥的值设置为 "`PerMonitorV2`" 来 Windows 窗体选择支持 DPI 感知。 如果启用了 DPI 感知，还会启用所有的 DPI 功能。 这些方法包括：

- DPI 更改的消息，这些消息由 `DisableDpiChangedMessageHandling` 项控制。

- 动态 DPI 支持，由 `EnableWindowsFormsHighDpiAutoResizing` 键控制。

- 单一传递控制缩放，由单个 <xref:System.Windows.Forms.Form> 控件的 `Form.DisableSinglePassControlScaling` 控制、定位控件的 `AnchorLayout.DisableSinglePassControlScaling` 键和 <xref:System.Windows.Forms.MonthCalendar> 控件的 `MonthCalendar.DisableSinglePassControlScaling` 键控制

- 高 DPI 缩放和布局改进，由 <xref:System.Windows.Forms.CheckedListBox> 控件的 `CheckListBox.DisableHighDpiImprovements` 键、<xref:System.Windows.Forms.DataGridView> 控件的 `DataGridView.DisableHighDpiImprovements` 键和 `Toolstrip.DisableHighDpiImprovements` 控件的 <xref:System.Windows.Forms.ToolStrip> 键控制。

通过将 `DpiAwareness` 设置为 `PerMonitorV2`，提供的单个默认选择设置通常适用于新的 Windows 窗体应用程序。 但是，通过将相应的键添加到应用程序配置文件中，可以选择不使用单个高 DPI 改进。 例如，若要利用除动态 DPI 支持之外的所有新 DPI 功能，请将以下内容添加到应用程序配置文件：

```xml
<System.Windows.Forms.ApplicationConfigurationSection>
   <add key="DpiAwareness" value="PerMonitorV2" />
   <!-- Disable dynamic DPI support -->
   <add key="EnableWindowsFormsHighDpiAutoResizing" value="false" />
</System.Windows.Forms.ApplicationConfigurationSection>
```

通常，您选择不使用特定功能，因为您已选择以编程方式对其进行处理。

若要详细了解 Windows 窗体应用程序中的高 DPI 支持，请参阅[Windows 窗体中的高 Dpi 支持](../../../winforms/high-dpi-support-in-windows-forms.md)。

### <a name="disabledpichangedmessagehandling"></a>DisableDpiChangedMessageHandling

从 .NET Framework 4.7 开始，Windows 窗体控件引发了许多与 DPI 缩放更改相关的事件。 其中包括 <xref:System.Windows.Forms.Control.DpiChangedAfterParent>、<xref:System.Windows.Forms.Control.DpiChangedBeforeParent>和 <xref:System.Windows.Forms.Form.DpiChanged> 事件。 `DisableDpiChangedMessageHandling` 键的值确定是否在 Windows 窗体应用程序中引发这些事件。

### <a name="single-pass-scaling"></a>单步缩放

单个或多个传递缩放会影响用户界面的感知响应能力和用户界面元素在缩放时的视觉外观。 从 .NET Framework 4.7 开始，Windows 窗体使用单个轮缩放。 在以前版本的 .NET Framework 中，缩放是通过多个走刀执行的，这会导致某些控件的缩放比例超出了必需。 仅当你的应用程序依赖于旧行为时，才应禁用单传递缩放。

## <a name="see-also"></a>请参阅

- [Windows 窗体配置节](index.md)
- [Windows 窗体中的高 DPI 支持](../../../winforms/high-dpi-support-in-windows-forms.md)
