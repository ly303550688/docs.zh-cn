---
title: 泛型和属性 - C# 编程指南
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], attributes
- attributes [C#], with generics
ms.assetid: da9fc326-4648-454a-8e13-3911a2edefd7
ms.openlocfilehash: 47bf4e8f07a8b6778db8fa675d745d362dc10d7d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "75703021"
---
# <a name="generics-and-attributes-c-programming-guide"></a>泛型和特性（C# 编程指南）
属性可按与非泛型类型相同的方式应用到泛型类型。 有关应用特性的详细信息，请参阅[属性](../concepts/attributes/index.md)。  
  
 仅允许自定义属性引用开放式泛型类型（即未向其提供任何类型参数的泛型类型）和封闭式构造泛型类型（即向所有类型参数提供参数的泛型类型）。  
  
 以下示例使用此自定义属性：  
  
 [!code-csharp[csProgGuideGenerics#48](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#48)]  
  
 属性可引用开放式泛型类型：  
  
 [!code-csharp[csProgGuideGenerics#49](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#49)]  
  
 通过使用适当数量的逗号指定多个类型参数。 在此示例中，`GenericClass2` 具有两个类型参数：  
  
 [!code-csharp[csProgGuideGenerics#50](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#50)]  
  
 属性可引用封闭式构造泛型类型：  
  
 [!code-csharp[csProgGuideGenerics#51](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#51)]  
  
 引用泛型类型参数的属性会导致编译时错误：  
  
 [!code-csharp[csProgGuideGenerics#52](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#52)]  
  
 不能从<xref:System.Attribute>继承泛型类型：  
  
 [!code-csharp[csProgGuideGenerics#53](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#53)]  
  
 若要在运行时获取有关泛型类型或类型参数的信息，可使用 <xref:System.Reflection> 方法。 有关详细信息，请参阅[泛型和反射](./generics-and-reflection.md)  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [泛型](./index.md)
- [特性](../../../standard/attributes/index.md)
