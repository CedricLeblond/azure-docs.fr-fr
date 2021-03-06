---
title: "Résoudre les problèmes de l’agent de sauvegarde Azure (agent MARS) | Microsoft Docs"
description: "Résoudre les problèmes liés à l’installation et l’inscription de l’agent de sauvegarde Azure"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
ms.openlocfilehash: d05b951277515f3100aefcfb06a17b661267cb37
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshooting-azure-backup-agent-configurationregistration-issues"></a>Résoudre les problèmes liés à la configuration et l’inscription de l’agent de sauvegarde Azure
## <a name="recommended-steps"></a>Étapes recommandées
Consultez les actions recommandées indiquées ci-dessous pour résoudre les erreurs correspondantes rencontrées lors de la configuration ou de l’inscription de l’agent de sauvegarde Azure.

## <a name="error-invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Erreur : informations d’identification du coffre fournies non valides. Cela signifie que le fichier est endommagé ou qu’il ne contient pas les dernières informations d’identification associées au service de récupération.
| Détails de l’erreur | Causes possibles | Action(s) recommandée(s) |
| ---     | ---     | ---    |
| **Erreur** </br> *Informations d’identification du coffre fournies non valides. Cela signifie que le fichier est endommagé ou qu’il ne contient pas les dernières informations d’identification associées au service de récupération. (ID : 34513)* | <ol><li> Les informations d’identification du coffre ne sont pas valides (c’est-à-dire qu’elles ont été téléchargées plus de 48 heures avant l’heure de l’inscription).<li>   L’agent de sauvegarde Azure ne peut pas télécharger un fichier temporaire dans le dossier Temp de Windows. <li>Les informations d’identification du coffre se trouvent sur un emplacement réseau. <li>TLS 1.0 est désactivé<li> Un serveur proxy configuré bloque la connexion <br> |  <ol><li>Télécharger les nouvelles informations d’identification du coffre<li>Accédez à Options Internet > Sécurité > Internet > cliquez sur Personnaliser le niveau > faites défiler jusqu’à la section Téléchargement de fichiers et sélectionnez Activer.<li>Vous devrez peut-être aussi ajouter le site à vos [sites de confiance](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Modifier les paramètres pour utiliser le proxy et fournir les détails du proxy <li> Faire correspondre la date et l’heure (UTC) avec celles de votre ordinateur<li>Accédez à C:/Windows/Temp et vérifiez s’il y a plus de 60 000 ou 65 000 fichiers (avec l’extension .tmp) et supprimez ces fichiers.<li>Vous pouvez tester cela en exécutant le package SDP sur le serveur. Si vous obtenez une erreur indiquant que les téléchargements de fichiers ne sont pas autorisés, vous pouvez être sûr qu’il existe un nombre de fichiers élevé dans le répertoire C:/Windows/Temp.<li>Assurez-vous que .NET Framework 4.6.2 est installé <li>Si vous avez désactivé TLS 1.0 en raison de la conformité avec PCI, consultez ce [lien pour la résolution des problèmes](https://support.microsoft.com/help/4022913). <li>Si vous avez un antivirus installé sur le serveur, excluez les fichiers suivants de l’analyse antivirus. <ol><li>CBengine.exe<li>CSC.exe (lié à .NET Framework. Il existe un CSC.exe par version .NET installée sur le serveur. Excluez tous les fichiers CSC.exe liés à toutes les versions de .NET Framework sur le serveur concerné.) <li>Emplacement du dossier temporaire ou du cache. <br>*L’emplacement par défaut du dossier temporaire ou le chemin d’accès à l’emplacement du cache est C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Erreur : L’agent Microsoft Azure Recovery Service n’a pas pu se connecter à la Sauvegarde Microsoft Azure

| Détails de l’erreur | Causes possibles | Action(s) recommandée(s) |
| ---     | ---     | ---    |
| **Erreur** </br><ol><li>*L’agent Microsoft Azure Recovery Service n’a pas pu se connecter à la Sauvegarde Microsoft Azure. (ID : 100050) Vérifiez vos paramètres réseau et assurez-vous que vous pouvez vous connecter à Internet*<li>*(407) Authentification proxy requise* |Le proxy bloque la connexion* |  <ol><li>Accédez à Options Internet > Sécurité > Internet > cliquez sur Personnaliser le niveau > faites défiler jusqu’à la section Téléchargement de fichiers et sélectionnez Activer.<li>Vous devrez peut-être aussi ajouter le site à vos [sites de confiance](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Modifier les paramètres pour utiliser le proxy et fournir les détails du proxy <li>Si vous avez un antivirus installé sur le serveur, excluez les fichiers suivants de l’analyse antivirus. <ol><li>CBengine.exe<li>(au lieu de dpmra.exe)<li>CSC.exe (lié à .NET Framework. Il existe un CSC.exe par version .NET installée sur le serveur. Excluez tous les fichiers CSC.exe liés à toutes les versions de .NET Framework sur le serveur concerné.) <li>Emplacement du dossier temporaire ou du cache <br>*L’emplacement par défaut du dossier temporaire ou le chemin d’accès à l’emplacement du cache est C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-failed-to-set-the-encryption-key-for-secure-backups"></a>Erreur : Échec de définition de la clé de chiffrement pour les sauvegardes sécurisées

| Détails de l’erreur | Causes possibles | Action(s) recommandée(s) |
| ---     | ---     | ---    |      
| **Erreur** </br>*Échec de définition de la clé de chiffrement pour les sauvegardes sécurisées. Échec de l’opération en cours en raison d’une erreur de service interne « Erreur d’entrée non valide ». Veuillez réessayer l’opération après un certain temps. Si le problème persiste, contactez le support technique Microsoft* |Le serveur est déjà inscrit auprès d’un autre coffre| Désinscrivez le serveur du coffre et réinscrivez-le.

## <a name="error-the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Erreur : L’activation n’a pas réussi. Échec de l’opération en cours en raison d’une erreur de service interne [0x1FC07]

| Détails de l’erreur | Causes possibles | Action(s) recommandée(s) |
| ---     | ---     | ---    |          
| **Erreur** </br><ol><li>*L’activation n’a pas réussi. Échec de l’opération en cours en raison d’une erreur de service interne [0x1FC07]. Veuillez réessayer l’opération après un certain temps. Si le problème persiste, contactez le support technique Microsoft* <li>*Erreur 34506. La phrase secrète de chiffrement stockée sur cet ordinateur n’est pas correctement configurée* | <li> Le dossier temporaire se situe sur un volume dont l’espace est insuffisant <li> Le dossier temporaire est incorrectement déplacé vers un autre emplacement <li> Le fichier OnlineBackup.KEK est manquant | <li>Déplacez le dossier temporaire ou l’emplacement du cache vers un volume avec un espace disponible équivalent à 5-10 % de la taille totale des données de sauvegarde. Consultez les étapes indiquées dans [cet article](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) pour déplacer correctement l’emplacement du cache.<li> Assurez-vous que le fichier OnlineBackup.KEK est présent <br>*L’emplacement par défaut du dossier temporaire ou le chemin d’accès à l’emplacement du cache est C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="need-help-contact-support"></a>Vous avez besoin d’aide ? Contacter le support technique
Si vous avez besoin d’aide, [contactez le support technique](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pour obtenir une prise en charge rapide de votre problème.

## <a name="next-steps"></a>Étapes suivantes
* Obtenir plus de détails sur [la sauvegarde de votre serveur Windows avec l’agent de sauvegarde Azure](tutorial-backup-windows-server-to-azure.md).
* Si vous avez besoin de restaurer une sauvegarde, utilisez cet article pour [restaurer des fichiers sur un ordinateur Windows](backup-azure-restore-windows-server.md).
