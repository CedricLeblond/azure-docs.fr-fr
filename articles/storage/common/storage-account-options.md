---
title: "Options liées au compte de stockage Azure | Microsoft Doc"
description: "Comprendre les options pour l’utilisation de Stockage Azure."
services: storage
documentationcenter: 
author: jirwin
manager: jwillis
editor: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: jirwin
ms.openlocfilehash: 7f07734433694999d38429ca264c58c5f3c619e1
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2017
---
# <a name="azure-storage-account-options"></a>Options de compte de stockage Azure

## <a name="overview"></a>Vue d'ensemble
Stockage Azure fournit trois options de compte distinctes, avec différentes tarifications et fonctionnalités prises en charge. Il est important que les utilisateurs évaluent ces différences afin de déterminer la meilleure option pour leurs applications.  Les trois options différentes sont les suivantes :

* Les comptes **Usage général v2 (GPv2)** possèdent les fonctionnalités les plus récentes et prennent en charge les objets BLOB, les fichiers, les files d’attente et les tables. Aujourd'hui, ces dernières fonctionnalités incluent la hiérarchisation au niveau de l’objet blob, le stockage archive, des limites de compte plus élevées et des événements de stockage. La tarification a été conçue pour fournir les prix au Go les plus bas et des prix de transaction compétitifs pour l’industrie.

* Les comptes de **stockage d’objets blob** fournissent toutes les fonctionnalités les plus récentes pour les objets blob de blocs, mais ils prennent uniquement en charge les objets blob de blocs.  La tarification est très largement similaire à celle des comptes Usage général v2. Nous encourageons la plupart des utilisateurs à utiliser les comptes Usage général v2 au lieu d’utiliser des comptes de stockage d’objets blob.

* Les comptes **Usage général v1 (GPv1)** permettent l’utilisation de tous les services de Stockage Azure, mais ils ne possèdent pas les fonctionnalités les plus récentes ou de la tarification au Go la plus basse. Par exemple, le stockage froid et archive ne sont pas pris en charge par GPv1.  La tarification est inférieure pour les transactions, les charges de travail avec une évolution élevée ou un taux de lecture élevé peuvent donc tirer parti de ce type de compte.

### <a name="changing-account-kind"></a>Modification du type de compte
Les utilisateurs peuvent à tout moment mettre à niveau un compte de stockage GPv1 ou d’objets blob vers un compte GPv2 via le portail, CLI ou PowerShell. Cette modification ne peut pas être annulée, et aucune autre modification n’est autorisée.

## <a name="general-purpose-v2"></a>Usage général v2
Les comptes **Usage général v2 (GPv2)** sont des comptes de stockage qui prennent en charge toutes les fonctionnalités pour tous les services de stockage, y compris les objets blob, les fichiers, les files d’attente et les tables. Pour les objets blob de blocs, vous pouvez choisir un niveau de stockage chaud ou froid au niveau du compte, ou bien un niveau de stockage chaud, froid ou archive au niveau du fichier blob, en fonction des modèles d’accès. Stockez les données utilisées fréquemment, peu fréquemment et rarement dans des niveaux de stockage respectivement archive, froid et chaud pour optimiser les coûts. N’importe quel compte GPv1 peut être mis à niveau vers un compte GPv2 dans le portail, CLI, ou PowerShell. Les comptes GPv2 prennent en charge toutes les API et fonctionnalités prises en charge dans les comptes de stockage d’objets blob et GPv1, partagent les fonctionnalités de durabilité, disponibilité, évolutivité et de performances élevées de ces types de compte.

Les comptes de stockage d’objets blob exposent l’attribut **Niveau d’accès** au niveau du compte, spécifiant le niveau de compte de stockage par défaut comme **Chaud** ou **Froid**. Le niveau de compte de stockage par défaut est appliqué à tout objet blob n’ayant aucun niveau explicitement défini au niveau de l’objet blob. Si le modèle d’utilisation de vos données est modifié, vous pouvez également basculer entre ces niveaux de stockage à tout moment. Le **niveau de stockage archive** peut être appliqué uniquement au niveau de l’objet blob.

> [!NOTE]
> La modification du niveau de stockage peut entraîner des frais supplémentaires. Consultez la section [Tarification et facturation](#pricing-and-billing) pour plus de détails.

## <a name="blob-storage-accounts"></a>Comptes de stockage d’objets blob

Les **comptes de stockage d’objets blob** prennent en charge les mêmes fonctionnalités d’objet blob de blocs que GPv2, mais ils sont limités à la prise en charge des objets blob de blocs. Les clients doivent examiner les différences de prix entre les comptes de stockage d’objets blob et GPv2 et considérer la mise à niveau vers GPv2. Notez que cette mise à niveau ne peut pas être annulée.

> [!NOTE]
> Les comptes de stockage d’objets blob prennent en charge uniquement les objets blob de blocs et les objets blob d’ajout, mais pas les objets blob de pages.

## <a name="general-purpose-v1"></a>Usage général v1
Les comptes **Usage général v1 (GPv1)** sont les comptes de stockage les plus anciens et les seuls pouvant être utilisés dans le modèle de déploiement classique. Les fonctionnalités telles que le stockage froid et archive ne sont pas disponibles avec les comptes GPv1. Les comptes GPv1 ont généralement des coûts de stockage par Go plus élevés, mais des coûts de transaction plus faibles que les comptes GPv2 ou de stockage d’objets blob.

## <a name="recommendations"></a>Recommandations

Pour plus d’informations sur les comptes de stockage, consultez [À propos des comptes de stockage Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) .

Pour les applications qui requièrent uniquement le stockage d’objets blob de blocs ou d’objets blob d’ajout, nous recommandons d’utiliser des comptes de stockage GPv2, pour tirer parti du modèle de tarification différencié du stockage hiérarchisé. Toutefois, nous comprenons que cela n’est pas possible dans certains cas, notamment lorsque l’utilisation de comptes de stockage GPv1 convient, par exemple :

* Vous devez toujours utiliser le modèle de déploiement classique. Les comptes de stockage d’objets blob sont uniquement disponibles via le modèle de déploiement Azure Resource Manager.

* Vous utilisez des volumes élevés de transactions ou de bande passante de géo-réplication, qui coûtent plus cher avec des comptes GPv2 et de stockage d’objets Blob qu’avec un compte GPv1 et vous n’avez pas suffisamment de stockage pour tirer bénéfice des réduction des coûts du stockage par Go.

* Vous utilisez une version de l’ [API REST des services de stockage](https://msdn.microsoft.com/library/azure/dd894041.aspx) antérieure à celle du 14/02/2014 ou une bibliothèque cliente avec une version inférieure à 4.x, et vous ne pouvez pas mettre à niveau votre application.

> [!NOTE]
> Les comptes de stockage d’objets blob sont actuellement pris en charge dans toutes les régions Azure.

## <a name="pricing-and-billing"></a>Tarification et facturation
Tous les comptes de stockage utilisent un modèle de tarification pour le stockage d’objets blob basé sur le niveau de chaque objet blob. Les considérations de facturation suivantes s’appliquent à l’utilisation des comptes de stockage :

* **Coûts de stockage**: les coûts de stockage de données varient selon la quantité de données stockées et le niveau de stockage. Le coût par gigaoctet diminue à mesure que le niveau refroidit.

* **Coûts d’accès aux données** : les frais d’accès aux données augmentent à mesure que le niveau refroidit. Pour les données des niveaux de stockage froid et archive, des frais d’accès aux données en lecture vous sont facturés par gigaoctet.

* **Coûts de transaction** : des frais par transaction pour tous les niveaux, augmentant à mesure que le niveau refroidit.

* **Coûts de transfert de données de géoréplication**: ces coûts s’appliquent uniquement aux comptes pour lesquels la géoréplication est configurée, y compris GRS et RA-GRS. Le transfert de données de géoréplication implique des frais par gigaoctet.

* **Coûts de transfert de données sortantes** : les transferts de données sortantes (données transférées hors d’une région Azure) sont facturés pour l’utilisation de la bande passante par gigaoctet. Cette facturation est cohérente avec les comptes de stockage à usage général.

* **Modification du niveau de stockage** : passer d’un niveau de stockage de compte froid à un niveau de stockage chaud implique des frais correspondant à la lecture de toutes les données existantes du compte de stockage. Toutefois, la modification du niveau de stockage de compte chaud vers un niveau de stockage froid induit des frais équivalents à l’écriture de toutes les données dans le niveau froid (comptes GPv2 uniquement).

> [!NOTE]
> Pour plus d’informations sur le modèle de tarification des comptes de stockage d’objets blob, consultez la page [Tarification du stockage Azure](https://azure.microsoft.com/pricing/details/storage/). Pour plus d’informations sur les frais qu’implique le transfert de données sortantes, consultez la page [Détails de la tarification – Transferts de données](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart-scenarios"></a>Scénarios de démarrage rapide

Dans cette section, les scénarios suivants sont décrits à l’aide du Portail Azure :

* Création d'un compte de stockage GPv2.
* Convertir un compte de stockage GPv1 ou de stockage d’objet blob en un compte de stockage GPv2.
* Comment définir le niveau de compte et d’objet blob dans un compte de stockage GPv2.

Vous ne pouvez pas définir le niveau d’accès archive dans les exemples suivants, car ce paramètre s’applique à l’ensemble du compte de stockage. Le niveau d’accès archive peut uniquement être défini sur un objet blob spécifique.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Créer un compte de stockage GPv2 dans le portail Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Dans le menu Hub, sélectionnez **Nouveau** > **Données et stockage** > **Compte de stockage**.

3. Entrez un nom pour votre compte de stockage.

    Il doit s’agir d’un nom global unique ; il fait partie de l’URL permettant d’accéder aux objets du compte de stockage.  

4. Sélectionnez **Resource Manager** comme modèle de déploiement.

    Le stockage hiérarchisé est uniquement utilisable avec des comptes de stockage Resource Manager ; ce modèle de déploiement est recommandé pour les nouvelles ressources. Pour plus d’informations, voir [Présentation d’Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. Dans la liste déroulante Type de compte, sélectionnez **Usage général v2**.

    Cette liste vous permet de sélectionner le type de compte de stockage. Le stockage hiérarchisé n’est pas disponible dans le stockage à usage général ; il l’est uniquement dans le type de compte de stockage d’objets blob.     

    Lorsque vous sélectionnez cette option, le niveau de performances est défini sur Standard. Le stockage hiérarchisé n’est pas disponible avec le niveau de performances Premium.

6. Sélectionnez l’option de réplication pour le compte de stockage : **LRS**, **GRS** ou **RA-GRS**. La valeur par défaut est **RA-GRS**.

    LRS = stockage localement redondant ; GRS = stockage géo-redondant (deux régions) ; RA-GRS = stockage géo-redondant avec accès en lecture (2 régions avec accès en lecture à la seconde).

    Pour plus d’informations sur les options de réplication d’Azure Storage, voir [Réplication Azure Storage](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Sélectionnez le niveau de stockage adapté à vos besoins : définissez le **Niveau d’accès** sur **Froid** ou **Chaud**. Le niveau par défaut est **Chaud**.

8. Sélectionnez l’abonnement dans lequel vous souhaitez créer le compte de stockage.

9. Spécifiez un nouveau groupe de ressources ou sélectionnez un groupe de ressources existant. Pour plus d’informations sur les groupes de ressources, consultez [Vue d’ensemble d’Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Sélectionnez la région de votre compte de stockage.

11. Cliquez sur **Créer** pour créer le compte de stockage.

### <a name="convert-a-gpv1-or-blob-storage-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Convertir un compte de stockage GPv1 ou de stockage d’objet blob en un compte de stockage GPv2 à l’aide du portail Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Pour accéder à votre compte de stockage, sélectionnez Toutes les ressources, puis sélectionnez votre compte de stockage.

3. Dans le panneau Paramètres, cliquez sur **Configuration**.

4. Sous le type de compte, cliquez sur **Mettre à niveau**.

5. Un nouveau panneau va apparaitre à droite pour confirmation. Sous Confirmer la mise à niveau, saisissez le nom de votre compte. 

5. Cliquez sur l’option Mettre à niveau figurant en bas du panneau.

### <a name="change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal"></a>Modifier le niveau de stockage d’un compte de stockage GPv2 à l’aide du Portail Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Pour accéder à votre compte de stockage, sélectionnez Toutes les ressources, puis sélectionnez votre compte de stockage.

3. Dans le panneau Paramètres, cliquez sur **Configuration** pour afficher et/ou modifier la configuration du compte.

4. Sélectionnez le niveau de stockage adapté à vos besoins : définissez le **Niveau d’accès** sur **Froid** ou **Chaud**.

5. Cliquez sur Enregistrer dans la partie supérieure du panneau.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>Modifier le niveau de stockage d’un objet blob à l’aide du Portail Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Pour accéder à votre objet blob dans votre compte de stockage, sélectionnez Toutes les ressources, puis votre compte de stockage et enfin sélectionnez votre objet blob.

3. Dans le panneau des propriétés de l’objet blob, cliquez sur le menu déroulant **Niveau d’accès** pour sélectionner niveau de stockage **Chaud**, **Froid**, ou **Archive**.

5. Cliquez sur Enregistrer dans la partie supérieure du panneau.

> [!NOTE]
> La modification du niveau de stockage peut entraîner des frais supplémentaires. Consultez la section [Tarification et facturation](#pricing-and-billing) pour plus de détails.


## <a name="evaluating-and-migrating-to-gpv2-storage-accounts"></a>Évaluation et migration vers des comptes de stockage GPv2
Cette section vise à aider les utilisateurs à effectuer une transition en douceur vers les comptes de stockage GPv2 (par opposition au GPv1). Il existe deux scénarios utilisateur :

* Vous disposez d’un compte de stockage GPv1 et envisagez de passer à un compte de stockage GPv2 avec le niveau de stockage approprié.
* Vous souhaitez utiliser un compte de stockage GPv2 ou vous disposez déjà d’un tel compte et souhaitez savoir si vous devez utiliser le niveau de stockage chaud ou froid.

Dans les deux cas, la première priorité est d’abord estimer les frais de stockage et d’accès aux données stockées dans un compte de stockage GPv2 pour les comparer avec vos frais actuels.

## <a name="evaluating-gpv2-storage-account-tiers"></a>Évaluation des niveaux de compte de stockage GPv2

Pour estimer le coût de stockage et d’accès aux données stockées dans un compte de stockage GPv2, vous devez évaluer votre modèle d’utilisation existant ou faire une estimation du modèle d’utilisation souhaité. En général, vous souhaitez connaître :

* Votre consommation de stockage : quel est le volume de données stockées et quelle est son évolution mensuelle ?

* Votre modèle d’accès au stockage : quel est le volume de données du compte faisant l’objet d’accès en lecture et en écriture (y compris les nouvelles données) ? Le nombre et le type de transactions utilisées pour accéder aux données.

## <a name="monitoring-existing-storage-accounts"></a>Analyse des comptes de stockage existants

Pour analyser vos comptes de stockage existants et rassembler ces informations, vous pouvez utiliser Azure Storage Analytics qui assure la journalisation et fournit les données de mesure d’un compte de stockage. Storage Analytics peut stocker des métriques qui comprennent les statistiques de transactions agrégées et les données de capacité relatives aux demandes adressées à un service de stockage GPv1, GPv2 et de stockage d’objet blob. Ces données sont stockées dans des tables connues dans le même compte de stockage.

Pour plus d’informations, consultez [À propos des métriques de Storage Analytics](https://msdn.microsoft.com/library/azure/hh343258.aspx) et [Schéma de table de métriques Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Les comptes de stockage d’objets blob exposent le point de terminaison du service de table uniquement pour le stockage et l’accès aux métriques associées à ce compte. Les comptes de stockage GPv1 ZRS ne gèrent pas les données métriques.

Pour analyser la consommation de stockage pour le service de stockage d’objets blob, vous devez activer les métriques de capacité.
Lorsque cette option est activée, les données de capacité sont enregistrées quotidiennement pour le service blob d’un compte de stockage comme une entrée de table écrite dans la table *$MetricsCapacityBlob* dans le même compte de stockage.

Pour analyser le modèle d’accès aux données pour le service de stockage d’objets blob, vous devez activer les métriques de transaction par heure au niveau de l’API. Lorsque cette option est activée, les transactions par API sont agrégées toutes les heures et enregistrées comme une entrée de table écrite dans la table *$MetricsHourPrimaryTransactionsBlob* dans le même compte de stockage. La table *$MetricsHourSecondaryTransactionsBlob* enregistre les transactions vers le point de terminaison secondaire lorsqu’il s’agit de comptes de stockage RA-GRS.

> [!NOTE]
> Ce processus d’estimation n’est pas applicable si vous avez un compte de stockage à usage général dans lequel vous avez stocké des objets blob de pages et des disques de machines virtuelles ou bien des files d’attente, des fichiers ou des tables, en même temps que des données d’objets blob de blocs et d’ajout. La raison est que les données de capacité ne distinguent pas les objets blob de blocs des autres types et il ne donne pas de données de capacité pour les autres types de données. Si vous utilisez ces types, une méthodologie alternative consiste à examiner les quantités sur votre plus récente facture.

Pour avoir une bonne estimation de votre consommation de données et de votre modèle d’accès, nous vous recommandons de sélectionner pour les métriques une période de rétention représentative de votre utilisation régulière et d’extrapoler. Une option consiste à conserver les données de métriques pendant sept jours et à collecter les données chaque semaine pour les analyser à la fin du mois. Une autre option consiste à conserver les données de métriques pendant les 30 derniers jours et à collecter et analyser les données à la fin de la période de 30 jours.

Pour plus d’informations sur l’activation, la collecte et l’affichage des données de métriques, voir [Activation des métriques de stockage Azure et affichage des données associées](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Le stockage, l’accès et le téléchargement des données d’analyse sont également facturés comme des données utilisateur standard.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Utilisation des mesures d’utilisation pour estimer les coûts

### <a name="storage-costs"></a>Coûts de stockage

La dernière entrée de la table de métriques de capacité *$MetricsCapacityBlob* avec la clé de ligne *'data'* affiche la capacité de stockage utilisée par les données utilisateur. La dernière entrée de la table de métriques de capacité *$MetricsCapacityBlob* avec la clé de ligne *'analytics'* affiche la capacité de stockage utilisée par les journaux d’analyse.

Cette capacité totale utilisée par les données utilisateur et les journaux d’analyse (si l’option est activée) permet ensuite d’estimer le coût de stockage des données dans le compte de stockage. La même méthode peut également être utilisée pour estimer les coûts de stockage dans les comptes de stockage GPv1.

### <a name="transaction-costs"></a>Coûts de transaction

La somme des entrées *'TotalBillableRequests'*d’une API dans la table de métriques de transaction indique le nombre total de transactions pour cette API. *Par exemple*, le nombre total de transactions *'GetBlob'* pendant une période donnée peut être calculé par la somme du total de demandes facturables pour toutes les entrées avec la clé de ligne *'user;GetBlob'*.

Pour estimer les frais de transaction pour les comptes de stockage d’objets blob, vous devez classer les transactions en trois groupes correspondant aux trois modèles de tarification.

* Les transactions d’écriture telles que *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* et *'CopyBlob'*.
* Les transactions de suppression telles que *'DeleteBlob'* et *'DeleteContainer'*.
* Toutes les autres transactions.

Pour estimer les frais de transaction pour les comptes de stockage GPv1, vous devez regrouper toutes les transactions, quelle que soit l’opération/l’API associée.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Coûts d’accès aux données et de transfert de données de géoréplication

La quantité de données lues et écrites dans un compte de stockage n’est pas fournie par Storage Analytics mais peut être estimée en consultant la table de métriques de transaction. La somme des entrées *'TotalIngress'* d’une API dans la table de métriques de transaction indique la quantité totale de données entrantes en octets pour cette API. De même, la somme des entrées *'TotalEgress'* indique la quantité totale des données sortantes en octets.

Pour estimer les coûts d’accès aux données pour les comptes de stockage d’objets blob, vous devez classer les transactions en deux groupes.

* La quantité de données récupérées à partir du compte de stockage peut être estimée en additionnant les entrées *'TotalEgress'* pour les opérations *'GetBlob'* et *'CopyBlob'*.

* La quantité de données écrites dans le compte de stockage peut être estimée en additionnant les entrées *'TotalIngress'* pour les opérations *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* et *'AppendBlock'*.

Le coût de transfert de données de géoréplication des comptes de stockage d’objets blob peut également être calculé en estimant la quantité de données écrites lors de l’utilisation d’un compte de stockage GRS ou RA-GRS.

> [!NOTE]
> Pour un exemple plus détaillé de calcul des coûts d’un niveau de stockage chaud ou froid, consultez l’article *« Que sont les niveaux Froid et Chaud et comment savoir lequel utiliser ? »* sur la [page relative à la tarification Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Migration des données existantes

Un compte GPv1 ou de stockage d’objets Blob peut être facilement mis à niveau vers un compte GPv2 sans temps d’arrêt ni modifications de l’API et sans nécessiter de déplacement des données. Il s’agit de l’un des principaux avantages des comptes GPv2 par rapport aux comptes de stockage d’objets blob.

Toutefois, si vous avez besoin de migrer vers un compte de stockage d’objets blob, vous pouvez suivre les instructions ci-dessous.

Un compte de stockage d’objets blob est un compte spécialisé pour stocker uniquement les objets blob de blocs et d’ajout. Les comptes de stockage à usage général existants, qui vous permettent également de stocker des tables, des files d’attente, des fichiers, des disques et des objets blob ne peuvent pas être convertis en comptes de stockage d’objets blob. Pour utiliser les niveaux de stockage, vous devez créer des comptes de stockage d’objets blob et migrer vos données existantes vers les comptes nouvellement créés.

Vous pouvez utiliser les méthodes suivantes pour migrer les données existantes vers les comptes de stockage d’objets blob à partir d’une solution de stockage local, d’un fournisseur de stockage cloud tiers ou de vos comptes de stockage à usage général existants dans Azure :

### <a name="azcopy"></a>AzCopy

AzCopy est un utilitaire de ligne de commande Windows conçu pour la copie de données hautes performances vers ou à partir d’Azure Storage. Vous pouvez utiliser AzCopy pour copier des données dans votre compte de stockage d’objets blob à partir de vos comptes de stockage à usage général existants, ou pour charger des données à partir de vos périphériques de stockage locaux vers votre compte de stockage d’objets blob.

Pour plus d’informations, voir [Transfert de données avec l’utilitaire de ligne de commande AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Bibliothèque de déplacement des données

La bibliothèque de déplacement de données Azure Storage pour .NET est basée sur l’infrastructure principale de déplacement de données sous-tendant AzCopy. La bibliothèque est conçue pour assurer des opérations de transfert de données fiables, simples et hautes performances, comme AzCopy. Cela vous permet de tirer pleinement parti des fonctionnalités offertes par AzCopy dans votre application de façon native, sans avoir à gérer l’exécution et la surveillance des instances externes d’AzCopy.

Pour plus d’informations, voir [Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>API REST ou bibliothèque cliente

Vous pouvez créer une application personnalisée pour migrer vos données vers un compte de stockage d’objets blob à l’aide de l’une des bibliothèques clientes Azure ou de l’API REST des services Azure Storage. Azure Storage offre des bibliothèques clientes enrichies pour une diversité de langages et plateformes, par exemple .NET, Java, C++, Node.JS, PHP, Ruby et Python. Les bibliothèques clientes offrent des fonctionnalités avancées telles que la logique de nouvelle tentative, la journalisation et les téléchargements parallèles. Vous pouvez également développer votre application directement avec l’API REST, qui peut être appelée à l’aide de n’importe quel langage permettant de créer des requêtes HTTP/HTTPS.

Pour plus d’informations, voir [Prise en main du stockage d’objets blob Azure](../blobs/storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Les objets blob chiffrés à l’aide du chiffrement côté client stockent les métadonnées relatives au chiffrement stockées avec l’objet blob. Il est absolument essentiel que n’importe quel mécanisme de copie s’assure de la préservation des métadonnées de blob et en particulier des métadonnées relatives au chiffrement. Si vous copiez des objets blob sans ces métadonnées, le contenu de l’objet blob ne peut plus être récupéré. Pour plus d’informations concernant les métadonnées liées au chiffrement, voir [Chiffrement côté client et Azure Key Vault pour Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="faq"></a>Forum Aux Questions

**Les comptes de stockage existants restent-ils disponibles ?**

Oui. Les comptes de stockage existants restent disponibles, avec les mêmes tarifs et fonctionnalités.  Ils ne permettent pas de choisir un niveau de stockage et n’offrent plus de possibilité de hiérarchisation.

**Quand et pourquoi dois-je commencer à utiliser des comptes de stockage GPv2 ?**

Les comptes de stockage GPv2 sont spécialisés pour fournir les coûts de stockage au Go les plus bas, tout en proposant des prix d’accès aux données et de transaction compétitifs pour l’industrie. Désormais, les comptes de stockage GPv2 représentent la méthode recommandée pour le stockage des objets blob. En effet, certaines fonctionnalités, telles que la notification des changements, seront introduites pour ce type de compte. Toutefois, la mise à niveau s’effectue au moment où vous le souhaitez, selon vos besoins.  Par exemple, vous pouvez choisir d’optimiser vos modèles de transaction avant d’effectuer la mise à niveau.

**Puis-je mettre à jour mon compte de stockage existant en un compte de stockage GPv2 ?**

Oui. Les comptes GPv1 ou de stockage d’objets blob peuvent facilement être mis à niveau vers un compte GPv2 depuis le portail.

**Puis-je stocker des objets dans les deux niveaux de stockage d’un même compte ?**

Oui. L’attribut **Niveau d’accès** configuré sur un niveau de compte est le niveau par défaut applicable à tous les objets du compte ne possédant par un niveau d’ensemble explicite. Toutefois, la hiérarchisation au niveau de l’objet blob permet de définir le niveau d’accès au niveau de l’objet, quel que soit le paramètre de niveau accès du compte. Des objets blob configurés sur l’un des trois niveaux de stockage (chaud, froid ou archive) peuvent exister dans le même compte.

**Puis-je modifier le niveau de stockage de mon compte de stockage GPv2 ?**

Oui, vous pouvez modifier le niveau de stockage de compte en définissant l’attribut **Niveau d’accès** du compte de stockage. La modification du niveau de stockage de compte s’applique à tous les objets stockés dans le compte et ne possédant pas un ensemble de niveau explicite. Passer d’un niveau de stockage chaud à un niveau de stockage froid implique des frais pour des opérations d’écriture (par 10 000) (comptes de stockage GPv2 uniquement), tandis que le passage d’un niveau froid à un niveau chaud entraîne des frais pour des opérations de lecture (par 10 000) et des extractions de données (par Go) pour la lecture de toutes les données dans le compte.

**À quelle fréquence puis-je modifier le niveau de stockage de mon compte de stockage d’objets blob ?**

Nous n’appliquons pas de limite concernant la fréquence de modification du niveau de stockage. Cependant, notez que le passage d’un niveau de stockage froid à un niveau chaud entraîne des frais importants. Il est déconseillé de modifier le niveau de stockage trop fréquemment.

**Les objets blob au niveau de stockage froid se comportent-ils différemment de ceux se trouvant au niveau de stockage chaud ?**

Les objets blob au niveau de stockage chaud des comptes de stockage GPv2 ou d’objets blob ont la même latence que les objets blob des comptes GPv1. Les objets blob au niveau de stockage froid ont une latence similaire (en millisecondes) à celle des objets blob au niveau chaud. Les objets blob dans le niveau de stockage archive ont une latence de plusieurs heures.

Les objets blob au niveau de stockage froid ont un contrat SLA de disponibilité légèrement inférieur à celui des objets blob stockés au niveau de stockage chaud. Pour plus d’informations, voir [SLA pour Storage](https://azure.microsoft.com/support/legal/sla/storage).

**Puis-je stocker des objets blob de pages et des disques de machine virtuelle dans les comptes de stockage d’objets blob ?**

Non. Les comptes de stockage d’objets blob prennent en charge uniquement les objets blob de blocs et les objets blob d’ajout, mais pas les objets blob de pages. Les disques de machine virtuelle Azure sont soutenus par des objets blob de pages. Par conséquent, les comptes de stockage d’objets blob ne peuvent pas être utilisés pour stocker des disques de machine virtuelle. Toutefois, il est possible de stocker des sauvegardes de disques de machine virtuelle sous forme d’objets blob de blocs dans un compte de stockage d’objets blob. Il s’agit de l’une des raisons pour envisager l’utilisation de comptes GPv2 au lieu des comptes de stockage d’objets blob.

**Dois-je modifier mes applications existantes pour utiliser des comptes de stockage GPv2 ?**

Les comptes de stockage GPv2 sont cohérents à 100 % au niveau des API avec les comptes GPv1 et de stockage d’objets Blob. Tant que votre application utilise des objets blob de blocs ou d’ajout, et que vous utilisez la version 2014-02-14 de [l’API REST Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) ou une version ultérieure, votre application doit fonctionner. Si vous utilisez une version antérieure du protocole, vous devez mettre à jour votre application pour utiliser la nouvelle version afin de travailler en toute transparence avec les deux types de comptes de stockage. En général, nous recommandons d’utiliser la dernière version, quel que soit le type de compte de stockage que vous utilisez.

**L’expérience utilisateur change-t-elle ?**

Les comptes de stockage GPv2 sont très similaires aux comptes de stockage GPv1 et héritent de toutes les fonctionnalités clés de Azure Storage, notamment de niveaux élevés de durabilité, disponibilité, évolutivité, performances et sécurité. Hormis les fonctionnalités et restrictions spécifiques aux comptes de stockage d’objets blob et aux niveaux de stockage correspondants évoqués plus haut, il n’existe aucune différence lors d’une mise à niveau vers un compte de stockage d’objets blob ou GPv2.

## <a name="next-steps"></a>Étapes suivantes

### <a name="evaluate-blob-storage-accounts"></a>Évaluer des comptes de stockage d’objets blob

[Vérifier la disponibilité de comptes de stockage d’objets blob par région](https://azure.microsoft.com/regions/#services)

[Évaluer l’utilisation des comptes de stockage actuels en activant les métriques Azure Storage](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Vérifier le prix du stockage d’objets blob par région](https://azure.microsoft.com/pricing/details/storage/)

[Vérifier la tarification des transferts de données](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>Commencer à utiliser des comptes de stockage GPv2

[Prise en main du stockage d’objets blob Azure](../blobs/storage-dotnet-how-to-use-blobs.md)

[Transfert de données vers et à partir d’Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Transfert de données avec l'utilitaire de ligne de commande AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Parcourez et explorez vos comptes de stockage](http://storageexplorer.com/)
