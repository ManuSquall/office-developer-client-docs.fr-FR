---
title: Création d'un profil à l'aide d'un code personnalisé
manager: soliver
ms.date: 11/16/2014
ms.audience: Developer
localization_priority: Normal
api_type:
- COM
ms.assetid: 5632cd25-58f5-4b9c-906c-cd377abc3daf
description: 'Dernière modification : 23 juillet 2011'
ms.openlocfilehash: f3270528194b3cc3429d3afec153355349dabbff
ms.sourcegitcommit: 8fe462c32b91c87911942c188f3445e85a54137c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "32335789"
---
# <a name="creating-a-profile-by-using-custom-code"></a><span data-ttu-id="5fecc-103">Création d'un profil à l'aide d'un code personnalisé</span><span class="sxs-lookup"><span data-stu-id="5fecc-103">Creating a Profile by Using Custom Code</span></span>

  
  
<span data-ttu-id="5fecc-104">**S’applique à** : Outlook 2013 | Outlook 2016</span><span class="sxs-lookup"><span data-stu-id="5fecc-104">**Applies to**: Outlook 2013 | Outlook 2016</span></span> 
  
<span data-ttu-id="5fecc-105">Si vous choisissez d'écrire du code pour créer un profil, assurez-vous que vous comprenez comment commander des entrées de profil, ainsi que le type et la quantité d'informations nécessaires pour chaque entrée.</span><span class="sxs-lookup"><span data-stu-id="5fecc-105">If you choose to write code to create a profile, make sure that you understand how to order profile entries and the type and amount of information that is needed for each entry.</span></span> <span data-ttu-id="5fecc-106">Les implications de la commande d'entrées dans un profil sont expliquées dans les [profils MAPI](mapi-profiles.md).</span><span class="sxs-lookup"><span data-stu-id="5fecc-106">The implications of ordering entries in a profile is explained in [MAPI Profiles](mapi-profiles.md).</span></span>
  
 <span data-ttu-id="5fecc-107">**Pour créer un profil avec du code C ou C++**</span><span class="sxs-lookup"><span data-stu-id="5fecc-107">**To create a profile with C or C++ code**</span></span>
  
1. <span data-ttu-id="5fecc-108">Lisez le fichier d'en-tête pour chaque service de messagerie.</span><span class="sxs-lookup"><span data-stu-id="5fecc-108">Read the header file for each message service.</span></span> <span data-ttu-id="5fecc-109">Comprenez les propriétés que vous devrez configurer et les valeurs que vous utiliserez.</span><span class="sxs-lookup"><span data-stu-id="5fecc-109">Understand what properties you will need to configure and what values you will use.</span></span>
    
2. <span data-ttu-id="5fecc-110">Appelez la fonction [MAPIAdminProfiles](mapiadminprofiles.md) pour récupérer un pointeur d'interface **IProfAdmin** .</span><span class="sxs-lookup"><span data-stu-id="5fecc-110">Call the [MAPIAdminProfiles](mapiadminprofiles.md) function to retrieve an **IProfAdmin** interface pointer.</span></span> 
    
3. <span data-ttu-id="5fecc-111">Appelez [IProfAdmin:: CreateProfile](iprofadmin-createprofile.md) pour créer votre profil.</span><span class="sxs-lookup"><span data-stu-id="5fecc-111">Call [IProfAdmin::CreateProfile](iprofadmin-createprofile.md) to create your profile.</span></span> <span data-ttu-id="5fecc-112">Si vous souhaitez créer un profil avec les services de messagerie figurant dans la section **[default services]** du MAPISVC. INF, définissez l'indicateur MAPI_DEFAULT_SERVICE.</span><span class="sxs-lookup"><span data-stu-id="5fecc-112">If you want to create a profile with the message services listed in the **[Default Services]** section of the MAPISVC.INF file, set the MAPI_DEFAULT_SERVICE flag.</span></span> <span data-ttu-id="5fecc-113">Si vous souhaitez permettre à l'utilisateur d'entrer des informations de configuration, définissez l'indicateur MAPI_DIALOG.</span><span class="sxs-lookup"><span data-stu-id="5fecc-113">If you want to enable the user to enter configuration information, set the MAPI_DIALOG flag.</span></span> <span data-ttu-id="5fecc-114">Veillez à définir cet indicateur si toutes les informations nécessaires ne sont pas disponibles via le MAPISVC. Fichier INF.</span><span class="sxs-lookup"><span data-stu-id="5fecc-114">Make sure that you set this flag if not all of the necessary information is available through the MAPISVC.INF file.</span></span> <span data-ttu-id="5fecc-115">**CreateProfile** appelle la fonction de point d'entrée pour chaque service de messagerie à ajouter au profil avec MSG_SERVICE_CREATE défini en tant que paramètre _ulContext_ .</span><span class="sxs-lookup"><span data-stu-id="5fecc-115">**CreateProfile** calls the entry point function for each message service to be added to the profile with MSG_SERVICE_CREATE set as the  _ulContext_ parameter.</span></span> 
    
4. <span data-ttu-id="5fecc-116">Appelez [IProfAdmin:: AdminServices](iprofadmin-adminservices.md) pour obtenir un objet d'administration de service de messagerie.</span><span class="sxs-lookup"><span data-stu-id="5fecc-116">Call [IProfAdmin::AdminServices](iprofadmin-adminservices.md) to obtain a message service administration object.</span></span> 
    
5. <span data-ttu-id="5fecc-117">Utilisez l'objet d'administration du service de messagerie pour ajouter des services de messagerie au profil.</span><span class="sxs-lookup"><span data-stu-id="5fecc-117">Use the message service administration object to add message services to the profile.</span></span> <span data-ttu-id="5fecc-118">Pour chaque service de messagerie que vous souhaitez ajouter:</span><span class="sxs-lookup"><span data-stu-id="5fecc-118">For each message service that you want to add:</span></span>
    
1. <span data-ttu-id="5fecc-119">Appelez la méthode [IMsgServiceAdmin:: CreateMsgService](imsgserviceadmin-createmsgservice.md) pour créer le nouveau service de messagerie.</span><span class="sxs-lookup"><span data-stu-id="5fecc-119">Call the [IMsgServiceAdmin::CreateMsgService](imsgserviceadmin-createmsgservice.md) method to create the new message service.</span></span> 
    
2. <span data-ttu-id="5fecc-120">Appelez [IMsgServiceAdmin:: ConfigureMsgService](imsgserviceadmin-configuremsgservice.md), en transmettant la structure **MAPIUID** du service que vous venez de créer et un tableau de valeurs de propriété avec ses propriétés de configuration.</span><span class="sxs-lookup"><span data-stu-id="5fecc-120">Call [IMsgServiceAdmin::ConfigureMsgService](imsgserviceadmin-configuremsgservice.md), passing the **MAPIUID** structure of the service you just created and a property value array with its configuration properties.</span></span> 
    
6. <span data-ttu-id="5fecc-121">Pour récupérer l'identificateur d'un service nouvellement ajouté, qui est sa propriété **PR_SERVICE_UID** ([PidTagServiceUid](pidtagserviceuid-canonical-property.md)), appelez [IMsgServiceAdmin:: GetMsgServiceTable](imsgserviceadmin-getmsgservicetable.md) pour accéder à la table de service de messages et rechercher la ligne qui représente service de messagerie.</span><span class="sxs-lookup"><span data-stu-id="5fecc-121">To retrieve the identifier of a newly added service, which is its **PR_SERVICE_UID** ([PidTagServiceUid](pidtagserviceuid-canonical-property.md)) property, call [IMsgServiceAdmin::GetMsgServiceTable](imsgserviceadmin-getmsgservicetable.md) to access the message service table and search for the row that represents the message service.</span></span> <span data-ttu-id="5fecc-122">La dernière ligne du tableau représente le dernier service de messagerie ajouté.</span><span class="sxs-lookup"><span data-stu-id="5fecc-122">The last row in the table will represent the most recently added message service.</span></span> 
    
<span data-ttu-id="5fecc-123">Pour créer un nouveau profil temporaire, appelez la méthode [IProfAdmin::D eleteprofile](iprofadmin-deleteprofile.md) immédiatement après avoir connecté.</span><span class="sxs-lookup"><span data-stu-id="5fecc-123">To make a new profile temporary, call the [IProfAdmin::DeleteProfile](iprofadmin-deleteprofile.md) method immediately after you log on.</span></span> <span data-ttu-id="5fecc-124">**DeleteProfile** marquera le nouveau profil comme supprimé tout en le rendant utilisable pendant la durée de la session.</span><span class="sxs-lookup"><span data-stu-id="5fecc-124">**DeleteProfile** will mark the new profile as deleted while making it usable for the duration of the session.</span></span> <span data-ttu-id="5fecc-125">Étant donné qu'elle ne sera pas incluse dans la table de profils de la session, les autres clients ne pourront pas l'utiliser.</span><span class="sxs-lookup"><span data-stu-id="5fecc-125">Because it will not be included in the session's profile table, other clients will be unable to use it.</span></span> 
  

