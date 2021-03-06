---
title: 如何：在管道中使用阻塞集合的数组
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, blocking collections in pipeline
ms.assetid: a39c7ec3-3ad7-4f4d-8fe4-b3e9dbabe2ed
ms.openlocfilehash: 397c438bacd1cfed1613efef61e9d7266d55ea47
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711254"
---
# <a name="how-to-use-arrays-of-blocking-collections-in-a-pipeline"></a>如何：在管道中使用阻塞集合的数组
下面的示例演示如何将 <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> 对象的数组与静态方法（如 <xref:System.Collections.Concurrent.BlockingCollection%601.TryAddToAny%2A> 和 <xref:System.Collections.Concurrent.BlockingCollection%601.TryTakeFromAny%2A>）配合使用，在组件之间实现快速而灵活的数据传输。  
  
## <a name="example"></a>示例  
 下面的示例演示基本管道实现，在此实现中，每个对象同时获取输入集合中的数据、转换该数据并将该数据传递到输出集合。  
  
 [!code-csharp[CDS_BlockingCollection#07](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example07.cs#07)]
 [!code-vb[CDS_BlockingCollection#07](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/bcpipeline.vb#07)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [线程安全集合](../../../../docs/standard/collections/thread-safe/index.md)
