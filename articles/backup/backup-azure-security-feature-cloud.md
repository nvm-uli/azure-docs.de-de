---
title: Sicherheitsfeatures für den Schutz von Cloudworkloads
description: In diesem Artikel wird erläutert, wie Sie mit den Azure Backup-Sicherheitsfunktionen für mehr Sicherheit für Ihre Sicherungen sorgen können.
ms.topic: conceptual
ms.date: 09/13/2019
ms.openlocfilehash: 0be85bf57510f575f238012b9bd1ef21e44e3cf1
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894027"
---
# <a name="security-features-to-help-protect-cloud-workloads-that-use-azure-backup"></a>Sicherheitsfeatures für den Schutz von Cloudworkloads mit Azure Backup

Die Sorgen bezüglich Sicherheitsproblemen wie Schadsoftware, Ransomware und Eindringlingen werden immer größer. Diese Sicherheitsprobleme können erhebliche Daten- und finanzielle Verluste mit sich bringen. Zum Schutz gegen solche Angriffe verfügt Azure Backup jetzt über Sicherheitsfeatures für den Schutz von Sicherungsdaten auch nach dem Löschen. Eines dieser Features ist das vorläufige Löschen. Beim vorläufigen Löschen werden die Sicherungsdaten 14 Tage länger aufbewahrt, damit das jeweilige Sicherungselement auch dann ohne Datenverluste wiederhergestellt werden kann, wenn ein böswilliger Akteur die Sicherung einer VM löscht oder die Sicherungsdaten versehentlich gelöscht werden. Für diese zusätzlichen 14 Tage der Aufbewahrung von Sicherungsdaten mit dem Status „Vorläufiges Löschen“ fallen für Kunden keine Kosten an.

> [!NOTE]
> Mit dem vorläufigen Löschen werden nur gelöschte Sicherungsdaten geschützt. Wenn eine VM ohne Sicherung gelöscht wird, bewirkt das Feature „Vorläufiges Löschen“ nicht, dass die Daten erhalten bleiben. Alle Ressourcen sollten per Azure Backup geschützt werden, um die vollständige Resilienz sicherzustellen.
>

## <a name="soft-delete"></a>Vorläufiges Löschen

### <a name="supported-regions"></a>Unterstützte Regionen

Vorläufiges Löschen wird derzeit in unterstützt in USA, Westen-Mitte; Asien, Osten; Kanada, Mitte; Kanada, Osten; Frankreich, Mitte; Frankreich, Süden; Südkorea, Mitte; Südkorea, Süden; Vereinigtes Königreich, Süden; Vereinigtes Königreich, Westen; Australien, Osten; Australien, Südosten; Europa, Norden; USA, Westen; USA, Westen 2; USA, Mitte; Asien, Südosten; USA, Norden-Mitte, USA, Süden-Mitte; Japan, Osten; Japan, Westen; Indien, Süden; Indien, Mitte; Indien, Westen; USA, Osten 2; Schweiz, Norden; Schweiz, Westen und allen nationalen Regionen.

### <a name="soft-delete-for-vms-using-azure-portal"></a>Vorläufiges Löschen für VMs über das Azure-Portal

1. Der Sicherungsvorgang muss beendet werden, um die Sicherungsdaten einer VM zu löschen. Navigieren Sie im Azure-Portal zu Ihrem Recovery Services-Tresor, klicken Sie mit der rechten Maustaste auf das Sicherungselement, und wählen Sie die Option **Sicherung beenden** aus.

   ![Screenshot: Sicherungselemente im Azure-Portal](./media/backup-azure-security-feature-cloud/backup-stopped.png)

2. Im folgenden Fenster erhalten Sie die Möglichkeit, die Sicherungsdaten zu löschen oder zu speichern. Wenn Sie die Option **Sicherungsdaten löschen** und dann **Sicherung beenden** wählen, wird die VM-Sicherung nicht endgültig gelöscht. Stattdessen werden die Sicherungsdaten 14 Tage lang im Zustand „Vorläufig gelöscht“ aufbewahrt. Wenn Sie die Option **Sicherungsdaten löschen** auswählen, wird eine E-Mail-Benachrichtigung an die konfigurierte E-Mail-ID gesendet. Hiermit wird der Benutzer darüber informiert, dass die Aufbewahrungsdauer für die Sicherungsdaten um 14 Tage verlängert wurde. Darüber hinaus wird am 12. Tag eine E-Mail-Benachrichtigung mit dem Hinweis gesendet, dass zwei weitere Tage verbleiben, um die gelöschten Daten wiederherzustellen. Das endgültige Löschen wird erst am 15. Tag durchgeführt, und es wird eine abschließende E-Mail-Benachrichtigung gesendet, um den Benutzer über die endgültige Löschung der Daten zu informieren.

   ![Screenshot: „Sicherung beenden“ im Azure-Portal](./media/backup-azure-security-feature-cloud/delete-backup-data.png)

3. Während dieser 14 Tage wird die vorläufig gelöschte VM im Recovery Services-Tresor mit einem roten Symbol gekennzeichnet, mit dem der Status „Vorläufig gelöscht“ angezeigt wird.

   ![Screenshot: Vorläufig gelöschte VM im Azure-Portal](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

   > [!NOTE]
   > Wenn im Tresor vorläufig gelöschte Sicherungselemente vorhanden sind, kann der Tresor nicht gelöscht werden. Versuchen Sie erst, den Tresor zu löschen, nachdem die Sicherungselemente endgültig gelöscht wurden und im Tresor kein Element mit dem Status „Vorläufig gelöscht“ mehr vorhanden ist.

4. Das Löschen muss zuerst wieder rückgängig gemacht werden, um die vorläufig gelöschte VM wiederherstellen zu können. Wählen Sie zum Rückgängigmachen des Löschvorgangs den vorläufig gelöschten virtuellen Computer und anschließend **Wiederherstellen** aus.

   ![Screenshot: Wiederherstellen der VM im Azure-Portal](./media/backup-azure-security-feature-cloud/choose-undelete.png)

   Es wird ein Fenster mit einer Warnung angezeigt, dass bei Auswahl der Option „Wiederherstellen“ alle Wiederherstellungspunkte für die VM wiederhergestellt werden und für die Durchführung eines Wiederherstellungsvorgangs verfügbar sind. Für die VM gilt der Status „Beendigung des Schutzes mit Beibehaltung der Daten“. Die Sicherungen sind angehalten, und die Sicherungsdaten werden ohne zeitliche Beschränkung aufbewahrt, ohne dass eine Sicherungsrichtlinie gilt.

   ![Screenshot: Bestätigung der VM-Wiederherstellung im Azure-Portal](./media/backup-azure-security-feature-cloud/undelete-vm.png)

   An diesem Punkt können Sie die VM auch wiederherstellen, indem Sie für den ausgewählten Wiederherstellungspunkt die Option **Virtuellen Computer wiederherstellen** auswählen.  

   ![Screenshot: Option „Virtuellen Computer wiederherstellen“ im Azure-Portal](./media/backup-azure-security-feature-cloud/restore-vm.png)

   > [!NOTE]
   > Der Garbage Collector zum Bereinigen abgelaufener Wiederherstellungspunkte wird erst ausgeführt, nachdem der Benutzer den Vorgang **Sicherung fortsetzen** durchgeführt hat.

5. Nachdem der Wiederherstellungsprozess abgeschlossen ist, wird der Status auf „Sicherung beenden mit Daten beibehalten“ zurückgesetzt, und anschließend können Sie die Option **Sicherung fortsetzen** auswählen. Mit **Sicherung fortsetzen** wird das Sicherungselement wieder in den aktiven Zustand versetzt, und es wird eine vom Benutzer ausgewählte Sicherungsrichtlinie zugeordnet, mit der die Sicherungs- und Aufbewahrungszeitpläne definiert werden.

   ![Screenshot: Option „Sicherung fortsetzen“ im Azure-Portal](./media/backup-azure-security-feature-cloud/resume-backup.png)

Im folgenden Flussdiagramm sind die unterschiedlichen Schritte und Zustände eines Sicherungselements bei aktiviertem vorläufigem Löschen dargestellt:

![Lebenszyklus eines vorläufig gelöschten Sicherungselements](./media/backup-azure-security-feature-cloud/lifecycle.png)

Weitere Informationen finden Sie unten im Abschnitt [Häufig gestellte Fragen](backup-azure-security-feature-cloud.md#frequently-asked-questions).

### <a name="soft-delete-for-vms-using-azure-powershell"></a>Vorläufiges Löschen für VMs über Azure PowerShell

> [!IMPORTANT]
> Die erforderliche Mindestversion von Az.RecoveryServices für die Verwendung des vorläufigen Löschens mit Azure PS ist 2.2.0. Verwenden Sie ```Install-Module -Name Az.RecoveryServices -Force```, um die aktuelle Version herunterzuladen.

Wie oben für das Azure-Portal erläutert, ist die Abfolge der Schritte bei der Verwendung von Azure PowerShell identisch.

#### <a name="delete-the-backup-item-using-azure-powershell"></a>Löschen des Sicherungselements mit Azure PowerShell

Löschen Sie das Sicherungselement mithilfe des PowerShell-Cmdlets [Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0).

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

Der DeleteState des Sicherungselements ändert sich von „NotDeleted“ in „ToBeDeleted“. Die Sicherungsdaten werden 14 Tage lang aufbewahrt. Wenn Sie den Löschvorgang rückgängig machen möchten, sollten Sie den Vorgang zum Rückgängigmachen des Löschens ausführen.

#### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>Rückgängigmachen des Löschvorgangs mithilfe von Azure PowerShell

Rufen Sie zuerst das relevante Sicherungselement ab, das sich im Zustand für vorläufiges Löschen befindet, der bedeutet, dass das Löschen dieses Elements bevorsteht.

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Führen Sie dann den Vorgang zum Rückgängigmachen des Löschens mithilfe des PowerShell-Cmdlets [Undo-AzRecoveryServicesBackupItemDeletion](https://docs.microsoft.com/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion?view=azps-3.1.0) aus.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Der DeleteState des Sicherungselements wird auf „NotDeleted“ zurückgesetzt. Der Schutz bleibt jedoch weiterhin unterbrochen. Sie müssen [die Sicherung fortsetzen](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#change-policy-for-backup-items), um den Schutz erneut zu aktivieren.

## <a name="disabling-soft-delete"></a>Deaktivieren des vorläufigen Löschens

Vorläufiges Löschen ist für neu erstellte Tresore standardmäßig aktiviert, um Sicherungsdaten vor versehentlichen oder vorsätzlichen Löschvorgängen zu schützen.  Es wird davon abgeraten, dieses Feature zu deaktivieren. Sie sollten das vorläufige Löschen nur dann deaktivieren, wenn Sie planen, Ihre geschützten Elemente in einen neuen Tresor zu verschieben, und nicht die 14 Tage warten können, die für das Löschen und erneute Schützen erforderlich sind (z. B. in einer Testumgebung). Dieses Feature kann nur durch einen Sicherungsadministrator deaktiviert werden. Wenn Sie dieses Feature deaktivieren, führen alle Löschvorgänge von geschützten Elementen zur sofortigen und endgültigen Entfernung der Elemente. Sicherungsdaten, die vor der Deaktivierung dieses Features vorläufig gelöscht wurden, verbleiben im vorläufigen Löschzustand. Wenn Sie diese Elemente umgehend endgültig löschen möchten, müssen Sie sie wiederherstellen und anschließend erneut löschen, damit sie endgültig gelöscht werden.

### <a name="disabling-soft-delete-using-azure-portal"></a>Deaktivieren des vorläufigen Löschens für VMs über das Azure-Portal

Gehen Sie zum Deaktivieren des vorläufigen Löschens wie folgt vor:

1. Wechseln Sie im Azure-Portal zu Ihrem Tresor und dann zu **Einstellungen** -> **Eigenschaften**.
2. Wählen Sie im Eigenschaftenbereich **Sicherheitseinstellungen** -> **Aktualisieren** aus.  
3. Wählen Sie im Bereich mit den Sicherheitseinstellungen unter **Vorläufiges Löschen** die Option **Deaktivieren** aus.

![Deaktivieren des vorläufigen Löschens](./media/backup-azure-security-feature-cloud/disable-soft-delete.png)

### <a name="disabling-soft-delete-using-azure-powershell"></a>Deaktivieren des vorläufigen Löschens für VMs über Azure PowerShell

> [!IMPORTANT]
> Die erforderliche Mindestversion von Az.RecoveryServices für die Verwendung des vorläufigen Löschens mit Azure PS ist 2.2.0. Verwenden Sie ```Install-Module -Name Az.RecoveryServices -Force```, um die aktuelle Version herunterzuladen.

Verwenden Sie zum Deaktivieren das PowerShell-Cmdlet [Set-AzRecoveryServicesVaultBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty?view=azps-3.1.0).

```powershell
Set-AzRecoveryServicesVaultProperty -VaultId $myVaultID -SoftDeleteFeatureState Disable


StorageModelType       :
StorageType            :
StorageTypeState       :
EnhancedSecurityState  : Enabled
SoftDeleteFeatureState : Disabled
```

## <a name="permanently-deleting-soft-deleted-backup-items"></a>Endgültiges Löschen vorläufig gelöschter Sicherungselemente

Sicherungsdaten, die vor der Deaktivierung dieses Features vorläufig gelöscht wurden, verbleiben im vorläufigen Löschzustand. Wenn Sie diese Elemente umgehend endgültig löschen möchten, müssen Sie sie wiederherstellen und anschließend erneut löschen, damit sie endgültig gelöscht werden.

### <a name="using-azure-portal"></a>Verwenden des Azure-Portals

Folgen Sie diesen Schritten:

1. Führen Sie die Schritte zum [Deaktivieren des vorläufigen Löschens](#disabling-soft-delete) aus. 
2. Navigieren Sie im Azure-Portal unter Ihrem Tresor zu **Sicherungselemente**, und wählen Sie den vorläufig gelöschten virtuellen Computer aus.

![Auswählen des vorläufig gelöschten virtuellen Computers](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

3. Wählen Sie die Option **Wiederherstellen** aus.

![Auswählen von „Wiederherstellen“](./media/backup-azure-security-feature-cloud/choose-undelete.png)


4. Daraufhin wird ein Fenster angezeigt. Wählen Sie **Wiederherstellen** aus.

![Auswählen von „Wiederherstellen“](./media/backup-azure-security-feature-cloud/undelete-vm.png)

5. Wählen Sie **Sicherungsdaten löschen** aus, um die Sicherungsdaten endgültig zu löschen.

![Auswählen von „Sicherungsdaten löschen“](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-buttom.png)

6. Geben Sie den Namen des Sicherungselements ein, um zu bestätigen, dass Sie die Wiederherstellungspunkte löschen möchten.

![Eingeben des Namens des Sicherungselements](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-data1.png)

7. Wählen Sie **Löschen** aus, um die Sicherungsdaten des Elements zu löschen. Sie erhalten einer Benachrichtigung, dass die Sicherungsdaten gelöscht wurden.

### <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell

Wenn Elemente vor der Deaktivierung des vorläufigen Löschens gelöscht wurden, befinden sie sich im vorläufig gelöschten Zustand. Um diese sofort zu löschen, muss der Löschvorgang umgekehrt und dann erneut ausgeführt werden.

Identifizieren Sie die Elemente, die sich im vorläufig gelöschten Zustand befinden.

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Kehren Sie dann den Löschvorgang um, der bei aktiviertem vorläufigem Löschen ausgeführt wurde.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Da das vorläufige Löschen jetzt deaktiviert ist, führt der Löschvorgang zu einem sofortigen Entfernen der Sicherungsdaten.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

## <a name="other-security-features"></a>Weitere Sicherheitsfeatures

### <a name="storage-side-encryption"></a>Speicherseitige Verschlüsselung

Azure Storage verschlüsselt Ihre Daten beim Speichern in der Cloud automatisch. Die Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Daten in Azure Storage werden auf transparente Weise mit der AES-256-Verschlüsselung – einer der stärksten verfügbaren Blockchiffren – ver- und entschlüsselt und sind mit dem FIPS 140-2-Standard konform. Die Azure Storage-Verschlüsselung ähnelt der BitLocker-Verschlüsselung unter Windows. Mit Azure Backup werden Daten vor dem Speichern automatisch verschlüsselt. Azure Storage entschlüsselt Daten vor dem Abrufen.  

In Azure werden Daten bei der Übertragung zwischen Azure Storage und dem Tresor per HTTPS geschützt. Diese Daten bleiben im Azure-Backbone-Netzwerk.

Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

### <a name="vm-encryption"></a>VM-Verschlüsselung

Sie können virtuelle Azure-Computer (VMs) unter Windows oder Linux mit verschlüsselten Datenträgern mithilfe des Diensts Azure Backup sichern und wiederherstellen. Anleitungen finden Sie unter [Sichern und Wiederherstellen verschlüsselter virtueller Computer mit Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

### <a name="protection-of-azure-backup-recovery-points"></a>Schutz von Azure Backup-Wiederherstellungspunkten

Von Recovery Services-Tresoren verwendete Speicherkonten sind isoliert, sodass böswillige Akteure keinen Zugriff darauf haben. Der Zugriff ist nur über Azure Backup-Verwaltungsvorgänge, z. B. eine Wiederherstellung, zulässig. Diese Verwaltungsvorgänge werden per rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC) gesteuert.

Weitere Informationen finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="soft-delete"></a>Vorläufiges Löschen

#### <a name="do-i-need-to-enable-the-soft-delete-feature-on-every-vault"></a>Muss ich die Funktion für vorläufiges Löschen in jedem Tresor aktivieren?

Nein. Sie wird standardmäßig für alle Recovery Services-Tresore erstellt und aktiviert.

#### <a name="can-i-configure-the-number-of-days-for-which-my-data-will-be-retained-in-soft-deleted-state-after-delete-operation-is-complete"></a>Kann ich konfigurieren, wie lange meine Daten nach Abschluss des Löschvorgangs im Status „Vorläufig gelöscht“ aufbewahrt werden?

Nein. Der Zeitraum ist auf eine um 14 Tage verlängerte Aufbewahrung nach dem Löschvorgang festgelegt.

#### <a name="do-i-need-to-pay-the-cost-for-this-additional-14-day-retention"></a>Muss ich die Kosten für diesen zusätzlichen Aufbewahrungszeitraum von 14 Tagen bezahlen?

Nein. Dieser zusätzliche Aufbewahrungszeitraum von 14 Tagen ist im Rahmen der Funktion „Vorläufiges Löschen“ kostenlos.

#### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-delete-state"></a>Kann ich einen Wiederherstellungsvorgang durchführen, wenn sich meine Daten im Status „Vorläufig gelöscht“ befinden?

Nein. Sie müssen das Löschen der vorläufig gelöschten Ressource rückgängig machen, um sie wiederherzustellen. Mit dem Vorgang „Wiederherstellen“ wird die Ressource zurück in den Status **Beendigung des Schutzes mit Beibehaltung der Daten** versetzt, und Sie können beliebige Wiederherstellungspunkte wiederherstellen. Der Garbage Collector bleibt in diesem Status angehalten.

#### <a name="will-my-snapshots-follow-the-same-lifecycle-as-my-recovery-points-in-the-vault"></a>Gilt für meine Momentaufnahmen der gleiche Lebenszyklus wie für meine Wiederherstellungspunkte im Tresor?

Ja.

#### <a name="how-can-i-trigger-the-scheduled-backups-again-for-a-soft-deleted-resource"></a>Wie kann ich die geplanten Sicherungen für eine vorläufig gelöschte Ressource erneut auslösen?

Wenn Sie nach dem Wiederherstellen den Vorgang „Fortsetzen“ ausführen, ist die Ressource wieder geschützt. Beim Vorgang „Fortsetzen“ wird eine Sicherungsrichtlinie zugeordnet, um die geplanten Sicherungen mit dem ausgewählten Aufbewahrungszeitraum auszulösen. Darüber hinaus wird der Garbage Collector ausgeführt, sobald der Vorgang „Fortsetzen“ abgeschlossen ist. Falls Sie eine Wiederherstellung für einen Wiederherstellungspunkt durchführen möchten, für den das Ablaufdatum überschritten wurde, sollten Sie dies tun, bevor Sie den Wiederherstellungsvorgang auslösen.

#### <a name="can-i-delete-my-vault-if-there-are-soft-deleted-items-in-the-vault"></a>Kann ich meinen Tresor löschen, wenn er vorläufig gelöschte Elemente enthält?

Der Recovery Services-Tresor kann nicht gelöscht werden, wenn sich darin Sicherungselemente im Zustand „Vorläufig gelöscht“ befinden. Die vorläufig gelöschten Elemente werden 14 Tage nach dem Löschvorgang endgültig gelöscht. Wenn Sie nicht so lange warten möchten, können Sie [vorläufiges Löschen deaktivieren](#disabling-soft-delete), die vorläufig gelöschten Elemente wiederherstellen und sie erneut löschen, um sie endgültig zu löschen. Nachdem Sie sich vergewissert haben, dass keine geschützten oder vorläufig gelöschten Elemente mehr vorhanden sind, kann der Tresor gelöscht werden.  

#### <a name="can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>Kann ich die Daten löschen, bevor die 14 Tage im Status „Vorläufig gelöscht“ abgelaufen sind?

Nein. Das Löschen der vorläufig gelöschten Elemente kann nicht erzwungen werden; sie werden nach 14 Tagen automatisch gelöscht. Diese Sicherheitsfunktion ist aktiviert, um die gesicherten Daten vor versehentlichen oder böswilligen Löschvorgängen zu schützen.  Sie sollten 14 Tage warten, bevor Sie eine andere Aktion auf dem virtuellen Computer ausführen.  Vorläufig gelöschte Elemente werden nicht in Rechnung gestellt.  Wenn Sie die zum vorläufigen Löschen gekennzeichneten VMs innerhalb von 14 Tagen in einem neuen Tresor erneut schützen müssen, wenden Sie sich an den Microsoft-Support.

#### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>Kann das vorläufige Löschen in PowerShell oder per CLI durchgeführt werden?

Vorläufige Löschvorgänge können mit [PowerShell](#soft-delete-for-vms-using-azure-powershell) durchgeführt werden. Die Befehlszeilenschnittstelle wird derzeit nicht unterstützt.

#### <a name="is-soft-delete-supported-for-other-cloud-workloads-like-sql-server-in-azure-vms-and-sap-hana-in-azure-vms"></a>Wird das vorläufige Löschen für andere Cloudworkloads unterstützt, z. B. SQL Server auf Azure-VMs und SAP HANA auf Azure-VMs?

Nein. Das vorläufige Löschen wird derzeit nur für virtuelle Azure-Computer unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Sicherheitskontrollen für Azure Backup](backup-security-controls.md).
