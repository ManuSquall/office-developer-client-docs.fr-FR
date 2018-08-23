---
title: Exemple de code de restriction
manager: soliver
ms.date: 11/16/2014
ms.audience: Developer
localization_priority: Normal
api_type:
- COM
ms.assetid: 9b82097c-dbd6-4ba0-a6cb-292301f9402b
description: 'Derniére modification : samedi 23 juillet 2011'
ms.openlocfilehash: dab13577e503a063ed1ebb48a3d6a5c531179b21
ms.sourcegitcommit: 0cf39e5382b8c6f236c8a63c6036849ed3527ded
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "22570260"
---
# <a name="sample-restriction-code"></a><span data-ttu-id="03202-103">Exemple de code de restriction</span><span class="sxs-lookup"><span data-stu-id="03202-103">Sample restriction code</span></span>

<span data-ttu-id="03202-104">**S’applique à**: Outlook 2013 | Outlook 2016</span><span class="sxs-lookup"><span data-stu-id="03202-104">**Applies to**: Outlook 2013 | Outlook 2016</span></span> 
  
<span data-ttu-id="03202-105">L’exemple de code suivant montre comment créer une restriction qui filtre tous les messages qui ne contiennent pas le mot « volleyball » dans la ligne d’objet et n’ont pas été envoyés à Sue SAM.</span><span class="sxs-lookup"><span data-stu-id="03202-105">The following sample code shows how to create a restriction that filters out all messages that do not contain the word "volleyball" in the subject line and were not sent to Sue from Sam.</span></span> <span data-ttu-id="03202-106">Une arborescence des structures [SRestriction](srestriction.md) est requise, avec le nœud supérieur en cours d’une restriction **et** implémentée avec une structure [SAndRestriction](sandrestriction.md) .</span><span class="sxs-lookup"><span data-stu-id="03202-106">A tree of [SRestriction](srestriction.md) structures is required, with the top node being an **AND** restriction implemented with an [SAndRestriction](sandrestriction.md) structure.</span></span> <span data-ttu-id="03202-107">Les trois restrictions qui sont joints à **l’opération** sont une restriction sous-objet qui recherche des messages envoyés à Sue, une restriction de contenu qui recherche des messages à partir de Sam et une autre restriction **et** recherche des messages qui disposent d’un objet contient « volleyball. »</span><span class="sxs-lookup"><span data-stu-id="03202-107">The three restrictions that are joined by the **AND** operation are a subobject restriction that searches for messages sent to Sue, a content restriction that searches for messages from Sam, and another **AND** restriction that searches for messages that have a subject containing "volleyball."</span></span> <span data-ttu-id="03202-108">Étant donné que **PR_SUBJECT** ([PidTagSubject](pidtagsubject-canonical-property.md)) n’est pas une propriété obligatoire, une restriction **existent** doit être incluse.</span><span class="sxs-lookup"><span data-stu-id="03202-108">Because **PR_SUBJECT** ([PidTagSubject](pidtagsubject-canonical-property.md)) is not a required property, an **Exist** restriction must be included.</span></span> 
  
<span data-ttu-id="03202-109">Ce code utilise l’allocation dynamique et l’initialisation ; Il est possible d’allouer et initialiser statiquement ainsi.</span><span class="sxs-lookup"><span data-stu-id="03202-109">This code uses dynamic allocation and initialization; it is possible to allocate and initialize statically as well.</span></span> <span data-ttu-id="03202-110">Dans un souci de concision, la vérification des erreurs qui doivent se produire en suivant les appels d’allocation ne figure pas dans l’exemple.</span><span class="sxs-lookup"><span data-stu-id="03202-110">In the interest of brevity, the error checking that must occur following the allocation calls is not included in the sample.</span></span> 
  
```cpp
HRESULT BuildRestriction (LPSTR pszSent, LPSTR pszFrom,
                                LPSTR pszSubjectText);
{
    LPSRestriction pRest, pAndRes, pObjRes, pSubjAndRes;
    LPSPropValue pRecip, pSender, pSubject;
    HRESULT hResult;
    ULONG ulResCount = 3, ulSubjCount = 2
    // Allocate and build  restriction to join criteria
    hResult = MAPIAllocateMore (sizeof(SRestriction)*ulResCount, pRest,
            (LPVOID *)&pAndRes);
    pRest->rt = RES_AND;
    pRest->res.resAnd.cRes = ulResCount;
    pRest->res.resAnd.lpRes = pAndRes;
    // Allocate and build subobject restriction to search recipient list
    hResult = MAPIAllocateMore (sizeof(SRestriction), pRest,
            (LPVOID *)&pObjRes);
    pAndRes[0].rt = RES_SUBRESTRICTION;
    pAndRes[0].res.resSub.ulSubObject = PR_MESSAGE_RECIPIENTS;
    pAndRes[0].res.resSub.lpRes = pObjRes;
    // Allocate and build content restriction to look for recipient
    hResult = MAPIAllocateMore (sizeof(SPropValue), pRest,
            (LPVOID *)&pRecip);
    pObjRes->rt = RES_CONTENT;
    pObjRes->res.resContent.ulFuzzyLevel =
            FL_FULLSTRING | FL_IGNORECASE;
    pObjRes->res.resContent.ulPropTag = pRecip->ulPropTag =
            PR_DISPLAY_NAME;
    pObjRes->res.resContent.lpProp = pRecip;
    pRecip->Value.LPSZ = pszSent;              // pszSent set to Sue
    // Allocate and build content restriction to look for sender
    hResult = MAPIAllocateMore (sizeof(SPropValue), pRest,
            (LPVOID *)&pSend);
    pAndRes[1].rt = RES_CONTENT;
    pAndRes[1].res.resContent.ulFuzzyLevel =
            FL_FULLSTRING | FL_IGNORECASE;
    pAndRes[1].res.resContent.ulPropTag = pSend->ulPropTag =
            PR_SENDER_NAME;
    pAndRes[1].res.resContent.lpProp = pSend;
    pSend->Value.LPSZ = pszName;                    // pszName set to Sam
    // Allocate and build  restriction to look for subject
    hResult = MAPIAllocateMore (sizeof(SRestriction)*ulSubjCount, pRest,
            (LPVOID *)&pSubjAndRes);
    pRest->rt = RES_AND;
    pRest->res.resAnd.cRes = ulResCount;
    pRest->res.resAnd.lpRes = pAndRes;
    // Create an  restriction to search for subject
    hResult = MAPIAllocateMore (sizeof(SPropValue), pRest,
            (LPVOID *)&pSubjAndRes);
    pAndRes[2].rt = RES_AND;
    pAndRes[2].res.resAnd.cRes = ulSubjCount;
    pAndRes[2].res.resAnd.lpRes = pSubjAndRes;
    // Exist restriction to check that PR_SUBJECT exists
    hResult = MAPIAllocateMore (sizeof(SPropValue), pRest,
            (LPVOID *)&pSubj);
    pSubjAndRes[0].rt = RES_EXIST;
    pSubjAndRes[0].res.resExist.ulReserved1 = 0;
    pSubjAndRes[0].res.resExist.ulReserved2 = 0;
    pSubjAndRes[0].res.resExist.ulPropTag = PR_SUBJECT;
    // Content restriction to check for "volleyball" in subject
    hResult = MAPIAllocateMore (sizeof(SPropValue), pRest,
            (LPVOID *)&pSubj);
    pSubjAndRes[1].res.resContent.ulFuzzyLevel =
            FL_SUBSTRING | FL_IGNORECASE;
    pSubjAndRes[1].res.resContent.ulPropTag = pSubj->ulPropTag =
            PR_SUBJECT;
    pSubjAndRes[1].res.resContent.lpProp = pSubj;
    pSubj->Value.LPSZ = pszSubjectText;
    return hResult;
}
 
```

## <a name="see-also"></a><span data-ttu-id="03202-111">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="03202-111">See also</span></span>

- [<span data-ttu-id="03202-112">Tables MAPI</span><span class="sxs-lookup"><span data-stu-id="03202-112">MAPI Tables</span></span>](mapi-tables.md)

