---
Exercise:
  title: "M01 – Lerneinheit\_6: Konfigurieren von DNS-Einstellungen in Azure"
  module: Module 01 - Introduction to Azure Virtual Networks
---

# M01 – Lerneinheit 6: Konfigurieren von DNS-Einstellungen in Azure

## Übungsszenario

In dieser Lerneinheit konfigurieren Sie die DNS-Namensauflösung für Contoso Ltd. Sie erstellen eine private DNS-Zone mit dem Namen „contoso.com“, verknüpfen die VNets für Registrierung und Auflösung, erstellen anschließend zwei VMs und testen die Konfiguration.

![Abbildung: DNS-Architektur.](../media/6-exercise-configure-domain-name-servers-configuration-azure.png)

In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen einer privaten DNS-Zone
+ Aufgabe 2: Verknüpfen des Subnetzes für automatische Registrierung
+ Aufgabe 3: Erstellen von VMs zum Testen der Konfiguration
+ Aufgabe 4: Überprüfen des Vorhandenseins von Einträgen in der DNS-Zone

   >**Hinweis:** Eine **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Configure%20DNS%20settings%20in%20Azure)** ist verfügbar, mit der Sie dieses Lab in Ihrem eigenen Tempo durcharbeiten können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und dem gehosteten Lab vor, aber die dargestellten Kernkonzepte und Ideen sind identisch.

### Geschätzte Dauer: 25 Minuten

## Aufgabe 1: Erstellen einer privaten DNS-Zone

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/).

1. Geben Sie auf der Azure-Homepage in der Suchleiste „dns“ ein, und wählen Sie dann **Private DNS-Zonen** aus.  
   ![Startseite des Azure-Portals mit DNS-Suche.](../media/create-private-dns-zone.png)

1. Klicken Sie unter „Private DNS-Zonen“ auf **+ Erstellen**.

1. Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen der privaten DNS-Zone:

    | **Tab**         | **Option**                             | **Wert**            |
    | --------------- | -------------------------------------- | -------------------- |
    | Grundlagen          | Resource group                         | ContosoResourceGroup |
    |                 | Name                                   | Contoso.com          |
    | Tags            | Keine Änderungen erforderlich                    |                      |
    | Bewerten + erstellen | Überprüfen Sie Ihre Einstellungen, und klicken Sie auf „Erstellen“. |                      |

1. Warten Sie, bis die Bereitstellung abgeschlossen ist, und wählen Sie dann **Zu Ressource wechseln** aus.

1. Vergewissern Sie sich, dass die Zone erstellt wurde.

## Aufgabe 2: Verknüpfen des Subnetzes für automatische Registrierung

1. Wählen Sie in „contoso.com“ unter **DNS-Verwaltung** den Eintrag **Verknüpfungen virtueller Netzwerke** aus.

1. Wählen Sie unter „contoso.com \| VNet-Verknüpfungen“ die Option **+ Hinzufügen** aus.

    ![contoso.com \| VNet-Verknüpfungen mit hervorgehobener Option „+ Hinzufügen“.](../media/add-network-link-dns.png)

1. Verwenden Sie die Informationen in der folgenden Tabelle zum Hinzufügen der VNet-Verknüpfung:

    | **Option**                          | **Wert**                               |
    | ----------------------------------- | --------------------------------------- |
    | Linkname                           | CoreServicesVnetLink                    |
    | Subscription                        | Keine Änderungen erforderlich                     |
    | Virtual Network                     | CoreServicesVnet (ContosoResourceGroup) |
    | Automatische Registrierung aktivieren            | Ausgewählt                                |
    | Überprüfen Sie Ihre Einstellungen, und klicken Sie auf „OK“. |                                         |

1. Wählen Sie **Aktualisieren** aus.

1. Vergewissern Sie sich, dass „CoreServicesVnetLink“ erstellt wurde und die automatische Registrierung aktiviert ist.

1. Wiederholen Sie die Schritte 2–5 für „ManufacturingVnet“ unter Verwendung der Informationen in der folgenden Tabelle:

    | **Option**                          | **Wert**                                |
    | ----------------------------------- | ---------------------------------------- |
    | Linkname                           | ManufacturingVnetLink                    |
    | Subscription                        | Keine Änderungen erforderlich                      |
    | Virtual Network                     | ManufacturingVnet (ContosoResourceGroup) |
    | Automatische Registrierung aktivieren            | Ausgewählt                                 |
    | Überprüfen Sie Ihre Einstellungen, und klicken Sie auf „OK“. |                                          |

1. Wählen Sie **Aktualisieren** aus.

1. Vergewissern Sie sich, dass „ManufacturingVnetLink“ erstellt wurde und die automatische Registrierung aktiviert ist.

1. Wiederholen Sie die Schritte 2–5 für „ResearchVnet“ unter Verwendung der Informationen in der folgenden Tabelle:

    | **Option**                          | **Wert**                           |
    | ----------------------------------- | ----------------------------------- |
    | Linkname                           | ResearchVnetLink                    |
    | Subscription                        | Keine Änderungen erforderlich                 |
    | Virtual Network                     | ResearchVnet (ContosoResourceGroup) |
    | Automatische Registrierung aktivieren            | Ausgewählt                            |
    | Überprüfen Sie Ihre Einstellungen, und klicken Sie auf „OK“. |                                     |

1. Wählen Sie **Aktualisieren** aus.

1. Vergewissern Sie sich, dass „ResearchVnetLink“ erstellt wurde und die automatische Registrierung aktiviert ist.

## Aufgabe 3: Erstellen von VMs zum Testen der Konfiguration

In diesem Abschnitt erstellen Sie zwei Test-VMs, um die Konfiguration der privaten DNS-Zone zu testen.

1. Wählen Sie im Azure-Portal das Cloud Shell-Symbol (oben rechts). Konfigurieren Sie die Shell bei Bedarf.  
    + Wählen Sie **PowerShell** aus.
    + Wählen Sie **Kein Speicherkonto erforderlich** und Ihr **Abonnement** aus und klicken Sie dann auf **Anwenden**.
    + Warten Sie, bis das Terminal erstellt wurde und eine Eingabeaufforderung angezeigt wird. 

1. Wählen Sie in der Symbolleiste des Cloud Shell-Bereichs das Symbol **Dateien verwalten** aus, wählen Sie im Dropdownmenü **Hochladen** aus und laden Sie die Dateien **azuredeploy.json** und **azuredeploy.parameters.json** nacheinander aus dem Quellordner **F:\Allfiles\Exercises\M01** in das Cloud Shell-Basisverzeichnis hoch.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderlichen VMs zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Stellen Sie sicher, dass beide VMs erstellt wurden.

## Aufgabe 4: Überprüfen des Vorhandenseins von Einträgen in der DNS-Zone

1. Wählen Sie auf der Startseite des Azure-Portals **Private DNS-Zonen** aus.

1. Wählen Sie unter „Private DNS-Zonen“ den Eintrag **contoso.com** aus.

1. Vergewissern Sie sich, dass Hosteinträge (A) für beide VMs aufgeführt werden, wie hier gezeigt:

    ![DNS-Zone „contoso.com“ mit Anzeige der automatisch registrierten Hosteinträge (A)](../media/contoso_com-dns-zone.png)

1. Notieren Sie sich die Namen und IP-Adressen der VMs.

### Herstellen einer Verbindung mit den Test-VMs unter Verwendung von RDP

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.

1. Wählen Sie **TestVM1** aus.

1. Wählen Sie unter TestVM1 **Verbinden &gt; RDP** aus, und laden Sie die RDP-Datei herunter.

    ![TestVM1 mit hervorgehobenen Optionen „Verbinden“ und „RDP“](../media/connect-to-am.png)

1. Speichern Sie die RDP-Datei auf Ihrem Desktop.

1. Führen Sie die gleichen Schritte für **TestVM2** aus

1. Stellen Sie eine Verbindung zu TestVM1 her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, die Sie bei der Bereitstellung angegeben haben.

1. Stellen Sie eine Verbindung zu TestVM2 her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, die Sie bei der Bereitstellung angegeben haben.

1. Wählen Sie für beide VMs unter **Wählen Sie die Datenschutzeinstellungen für Ihr Gerät aus** die Option **Akzeptieren** aus.

1. Wählen Sie auf beiden VMs, wenn Sie dazu aufgefordert werden, unter **Netzwerke** die Option **Ja** aus.

1. Öffnen Sie auf TestVM1 eine Eingabeaufforderung, und geben Sie den Befehl `ipconfig /all` ein.

1. Vergewissern Sie sich, dass die IP-Adresse mit der IP-Adresse identisch ist, die Sie für die DNS-Zone notiert haben.

1. Geben Sie den Befehl „ping TestVM2.contoso.com“ ein.

1. Überprüfen Sie, ob der FQDN zu der IP-Adresse aufgelöst wird, die Sie in der privaten DNS-Zone notiert haben. Beim Ping selbst zu einem Timeout, da Windows Defender Firewall auf den VMs aktiviert ist.

1. Alternativ können Sie den Befehl „nslooku TestVM2.contoso.com“ eingeben und überprüfen, ob Sie einen erfolgreichen Namensauflösungsdatensatz für VM2 erhalten.

## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Was ist der Unterschied zwischen Azure DNS und Azure Private DNS? Geben Sie Beispiele für die Verwendung von Azure Private DNS.
+ Welchen Zweck hat die automatische Registrierung bei der Erstellung einer Azure DNS-Zone?

## Weiterlernen im eigenen Tempo

+ [Einführung in Azure DNS](https://learn.microsoft.com/training/modules/intro-to-azure-dns/) In diesem Modul wird erläutert, was Azure DNS ist, wie es funktioniert und wann Sie Azure DNS als Lösung verwenden sollten, um die Anforderungen Ihrer Organisation zu erfüllen.
+ [Hosten Ihrer Domäne in Azure DNS.](https://learn.microsoft.com/training/modules/host-domain-azure-dns/) In diesem Modul erstellen Sie eine DNS-Zone und DNS-Einträge, um die Domain einer IP-Adresse zuzuordnen. Sie testen auch, ob der Domänenname auf Ihrem Webserver aufgelöst werden kann.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure DNS ist ein Clouddienst, mit dem Sie DNS-Domänen (Domain Name System) hosten und verwalten können, die auch als DNS-Zonen bezeichnet werden. 
+ Öffentliche Azure DNS-Zonen hosten Daten zu Domänennamenszonen für Einträge, die von jedem Host im Internet aufgelöst werden sollen.
+ Azure Private DNS-Zonen ermöglichen Ihnen die Konfiguration eines privaten DNS-Zonen-Namensraums für private Azure-Ressourcen.
+ Eine DNS-Zone ist eine Sammlung von DNS-Einträgen. DNS-Einträge liefern Informationen über die Domäne.
