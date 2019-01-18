---
title: Gestion des erreurs dans VBScript
TOCTitle: Handling errors in VBScript
ms:assetid: df8f96d5-b917-ddac-d274-6345b2499bf1
ms:mtpsurl: https://msdn.microsoft.com/library/JJ250135(v=office.15)
ms:contentKeyID: 48548222
ms.date: 09/18/2015
mtps_version: v=office.15
localization_priority: Normal
ms.openlocfilehash: 3f3da0d9e68024d7da9a0bcfca0fef30c7f646ed
ms.sourcegitcommit: d6695c94415fa47952ee7961a69660abc0904434
ms.translationtype: Auto
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2019
ms.locfileid: "28706729"
---
# <a name="handling-errors-in-vbscript"></a><span data-ttu-id="59048-102">Gestion des erreurs dans VBScript</span><span class="sxs-lookup"><span data-stu-id="59048-102">Handling errors in VBScript</span></span>


<span data-ttu-id="59048-103">**S’applique à**: Access 2013, Office 2013</span><span class="sxs-lookup"><span data-stu-id="59048-103">**Applies to**: Access 2013, Office 2013</span></span>

<span data-ttu-id="59048-104">Il y a peu de différences entre les méthodes Visual Basic et VBScript.</span><span class="sxs-lookup"><span data-stu-id="59048-104">There is little difference between the methods used in Visual Basic and those used with VBScript.</span></span> <span data-ttu-id="59048-105">La principale différence réside dans le fait que VBScript ne prend pas en charge le concept de gestion des erreurs en continuant l'exécution au niveau d'une étiquette.</span><span class="sxs-lookup"><span data-stu-id="59048-105">The primary difference is that VBScript does not support the concept of error handling by continuing execution at a label.</span></span> <span data-ttu-id="59048-106">En d'autres termes, vous ne pouvez pas utiliser On Error GoTo dans VBScript.</span><span class="sxs-lookup"><span data-stu-id="59048-106">In other words, you cannot use On Error GoTo in VBScript.</span></span> <span data-ttu-id="59048-107">À la place, utilisez-le dans VBScript.</span><span class="sxs-lookup"><span data-stu-id="59048-107">Instead, use in VBScript.</span></span> <span data-ttu-id="59048-108">Au lieu de cela, utilisez On Error Resume Next et vérifiez à la fois **Err.Number** et la propriété **Count** de la collection **Errors** , comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="59048-108">Instead, use On Error Resume Next and then check both **Err.Number** and the **Count** property of the **Errors** collection, as shown in the following example:</span></span>

```vb 
 
<!-- BeginErrorExampleVBS --> 
<HTML> 
<HEAD> 
<META NAME="GENERATOR" Content="Microsoft Visual Studio 6.0"> 
<TITLE>Error Handling example (VBScript)</TITLE> 
</HEAD> 
<BODY> 
 
<h1>Error Handling example (VBScript)</h1> 
 
<% 
 Dim errLoop 
 Dim strError 
 
 On Error Resume Next 
 
 ' Intentionally trigger an error. 
 Set cnn1 = Server.CreateObject("ADODB.Connection") 
 cnn1.Open "nothing" 
 
 If cnn1.Errors.Count > 0 Then 
 ' Enumerate Errors collection and display 
 ' properties of each Error object. 
 For Each errLoop In cnn1.Errors 
 strError = "Error #" & errLoop.Number & "<br>" & _ 
 " " & errLoop.Description & "<br>" & _ 
 " (Source: " & errLoop.Source & ")" & "<br>" & _ 
 " (SQL State: " & errLoop.SQLState & ")" & "<br>" & _ 
 " (NativeError: " & errLoop.NativeError & ")" & "<br>" 
 If errLoop.HelpFile = "" Then 
 strError = strError & _ 
 " No Help file available" & _ 
 "<br><br>" 
 Else 
 strError = strError & _ 
 " (HelpFile: " & errLoop.HelpFile & ")" & "<br>" & _ 
 " (HelpContext: " & errLoop.HelpContext & ")" & _ 
 "<br><br>" 
 End If 
 Response.Write ("<p>" & strError & "</p>") 
 Next 
 End If 
%> 
 
</BODY> 
</HTML> 
<!-- EndErrorExampleVBS --> 
```

