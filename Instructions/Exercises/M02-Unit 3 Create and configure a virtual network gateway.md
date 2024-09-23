---
Exercise:
  title: "M02 – Lerneinheit\_3: Erstellen und Konfigurieren eines Gateways des virtuellen Netzwerks"
  module: Module 02 - Design and implement hybrid networking
---


# M02 – Lerneinheit 3: Erstellen und Konfigurieren eines Gateways des virtuellen Netzwerks

## Übungsszenario

In dieser Übung konfigurieren Sie ein Gateway für virtuelle Netzwerke, um das Contoso Core Services-VNet und das Fertigungs-VNet zu verbinden.

![Abbildung eines VNet-Gateways](../media/3-exercise-create-configure-local-network-gateway.png)

In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen von CoreServicesVnet und ManufacturingVnet
+ Aufgabe 2: Erstellen von CoreServicesVM
+ Aufgabe 3: Erstellen von ManufacturingVM
+ Aufgabe 4: Herstellen einer Verbindung mit den VMs unter Verwendung von RDP
+ Aufgabe 5: Testen der Verbindung zwischen den VMs
+ Aufgabe 6: Erstellen des CoreServicesVnet-Gateways
+ Aufgabe 7: Erstellen des ManufacturingVnet-Gateways
+ Aufgabe 8: Verbinden von CoreServicesVnet mit ManufacturingVnet
+ Aufgabe 9: Verbinden von ManufacturingVnet mit CoreServicesVnet
+ Aufgabe 10: Überprüfen, ob die Verbindungen hergestellt werden
+ Aufgabe 11: Testen der Verbindung zwischen den VMs

**Hinweis:** Eine **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20a%20virtual%20network%20gateway)** ist verfügbar, mit der Sie dieses Lab in Ihrem eigenen Tempo durcharbeiten können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und dem gehosteten Lab vor, aber die dargestellten Kernkonzepte und Ideen sind identisch.

### Geschätzte Dauer: 70 Minuten (einschließlich ca. 45 Minuten Wartezeit bei der Bereitstellung)

## Aufgabe 1: Erstellen von CoreServicesVnet und ManufacturingVnet

1. Wählen Sie im Azure-Portal das Cloud Shell-Symbol (oben rechts). Konfigurieren Sie die Shell bei Bedarf.  
    + Wählen Sie **PowerShell** aus.
    + Wählen Sie **Kein Speicherkonto erforderlich** und Ihr **Abonnement** aus und klicken Sie dann auf **Anwenden**.
    + Warten Sie, bis das Terminal erstellt wurde und eine Eingabeaufforderung angezeigt wird. 

1. Wählen Sie in der Symbolleiste des Cloud Shell-Bereichs das Symbol **Dateien hochladen/herunterladen**, wählen Sie im Dropdownmenü die Option **Hochladen** und laden Sie die folgenden Dateien **azuredeploy.json** und **azuredeploy.parameters.json** nacheinander aus dem Quellordner **F:\Allfiles\Exercises\M02** in das Cloud Shell-Basisverzeichnis hoch.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um das virtuelle Netzwerk und die Subnetze zu erstellen, die für diese Übung erforderlich sind:

   ```powershell
   $RGName = "ContosoResourceGroup"
   #create resource group if it doesnt exist
   New-AzResourceGroup -Name $RGName -Location "eastus"
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```

 > **Hinweis:** Derzeit besteht ein Problem in der Region „Westeuropa, das Gatewaybereitstellungen beeinträchtigt. Als Abhilfemaßnahme wurde die Region „ManufacturingVnet“ für diese Bereitstellung in „Europa, Norden“ geändert.

## Aufgabe 2: Erstellen von CoreServicesVM

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Wählen Sie in der Symbolleiste des Cloud Shell-Bereichs das Symbol **Dateien hochladen/herunterladen**, wählen Sie im Dropdownmenü die Option **Hochladen** und laden Sie die folgenden Dateien **CoreServicesVMazuredeploy.json** und **CoreServicesVMazuredeploy.parameters.json** nacheinander aus dem Quellordner **F:\Allfiles\Exercises\M02** in das Cloud Shell-Basisverzeichnis hoch.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderlichen VMs zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile CoreServicesVMazuredeploy.json -TemplateParameterFile CoreServicesVMazuredeploy.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Überprüfen Sie, ob der virtuelle Computer erstellt wurde.

## Aufgabe 3: Erstellen von ManufacturingVM

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Wählen Sie in der Symbolleiste des Cloud Shell-Bereichs das Symbol **Dateien hochladen/herunterladen**, wählen Sie im Dropdownmenü die Option **Hochladen** und laden Sie die folgenden Dateien **ManufacturingVMazuredeploy.json** und **ManufacturingVMazuredeploy.parameters.json** nacheinander aus dem Quellordner **F:\Allfiles\Exercises\M02** in das Cloud Shell-Basisverzeichnis hoch.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderlichen VMs zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Überprüfen Sie, ob der virtuelle Computer erstellt wurde.

## Aufgabe 4: Herstellen einer Verbindung mit den VMs unter Verwendung von RDP

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.
1. Wählen Sie **ManufacturingVM** aus.
1. Wählen Sie unter **ManufacturingVM** die Option **Verbinden &gt; RDP** aus.
1. Wählen Sie unter **ManufacturingVM | Verbinden**, die Option **RDP-Datei herunterladen** aus.
1. Speichern Sie die RDP-Datei auf Ihrem Desktop.
1. Stellen Sie eine Verbindung zu **ManufacturingVM** her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, das Sie bei der Bereitstellung angegeben haben. Minimieren Sie nach der Verbindung die RDP-Sitzung.
1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.
1. Wählen Sie **CoreServicesVM** aus.
1. Wählen Sie auf **CoreServicesVM** die Option **Verbinden &gt; RDP** aus.
1. Wählen Sie auf **CoreServicesVM | Verbinden** die Option **RDP-Datei herunterladen** aus.
1. Speichern Sie die RDP-Datei auf Ihrem Desktop.
1. Stellen Sie eine Verbindung zu **CoreServicesVM** her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, das Sie bei der Bereitstellung angegeben haben.
1. Wählen Sie für beide VMs unter **Wählen Sie die Datenschutzeinstellungen für Ihr Gerät aus** die Option **Akzeptieren** aus.
1. Wählen Sie für beide VMs unter **Netzwerke** die Option **Ja** aus.
1. Öffnen Sie auf **CoreServicesVM** PowerShell, und führen Sie den folgenden Befehl aus: ipconfig.
1. Notieren Sie die IPv4-Adresse.

## Aufgabe 5: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf **ManufacturingVM** PowerShell.

1. Prüfen Sie mit dem folgenden Befehl, dass keine Verbindung zu CoreServicesVM auf CoreServicesVnet besteht. Achten Sie darauf, die IPv4-Adresse für CoreServicesVM zu verwenden.

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

1. Die Testverbindung sollte fehlschlagen, und es wird ein ähnliches Ergebnis wie das folgende zu sehen sein:

   ![Test-NetConnection fehlgeschlagen.](../media/test-netconnection-fail.png)

## Aufgabe 6: Erstellen des CoreServicesVnet Gateway

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.
   ![Suchen Sie im Azure-Portal nach „Gateway für virtuelle Netzwerke“.](../media/virtual-network-gateway-search.png)

1. Wählen Sie unter Gateways für virtuelle Netzwerke die Option **+ Erstellen** aus.

1. Verwenden Sie die Informationen in der folgenden Tabelle, um das Gateway für virtuelle Netzwerke zu erstellen.

   | **TAB**         | **Bereich**       | **Option**                                  | **Wert**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | Grundlagen          | Projektdetails   | Subscription                                | Keine Änderungen erforderlich          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | Instanzdetails  | Name                                        | CoreServicesVnetGateway      |
   |                 |                   | Region                                      | USA, Osten                      |
   |                 |                   | Gatewaytyp                                | VPN                          |
   |                 |                   | VPN-Typ                                    | routenbasiert                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation 1                  |
   |                 |                   | Virtuelles Netzwerk                             | CoreServicesVnet             |
   |                 |                   | Subnet                                      | GatewaySubnet (10.20.0.0/27) |
   |                 |                   | Typ der öffentlichen IP-Adresse                      | Standard                     |
   |                 | Öffentliche IP-Adresse | Öffentliche IP-Adresse                           | Neu erstellen                   |
   |                 |                   | Name der öffentlichen IP-Adresse                      | CoreServicesVnetGateway-ip   |
   |                 |                   | Aktiv/Aktiv-Modus aktivieren                   | Disabled                     |
   |                 |                   | Configure BGP (BGP konfigurieren)                               | Deaktiviert                     |
   | Überprüfen + erstellen |                   | Überprüfen Sie die Einstellungen, und wählen Sie **Erstellen** aus. |                              |

   > [!NOTE]
   >
   > Die Erstellung eines Gateways für das virtuelle Netzwerk kann bis zu 45 Minuten dauern.

## Aufgabe 7: Erstellen des ManufacturingVnet Gateway

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie unter Gateways für virtuelle Netzwerke die Option **+ Erstellen** aus.

1. Verwenden Sie die Informationen in der folgenden Tabelle, um das Gateway für virtuelle Netzwerke zu erstellen.

   | **TAB**         | **Bereich**       | **Option**                                  | **Wert**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | Grundlagen          | Projektdetails   | Subscription                                | Keine Änderungen erforderlich          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | Instanzdetails  | Name                                        | ManufacturingVnetGateway     |
   |                 |                   | Region                                      | Nordeuropa                  |
   |                 |                   | Gatewaytyp                                | VPN                          |
   |                 |                   | VPN-Typ                                    | routenbasiert                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation 1                  |
   |                 |                   | Virtuelles Netzwerk                             | ManufacturingVnet            |
   |                 |                   | Subnet                                      | GatewaySubnet (10.30.0.0/27) |
   |                 |                   | Typ der öffentlichen IP-Adresse                      | Standard                     |
   |                 | Öffentliche IP-Adresse | Öffentliche IP-Adresse                           | Neu erstellen                   |
   |                 |                   | Name der öffentlichen IP-Adresse                      | ManufacturingVnetGateway-ip  |
   |                 |                   | Aktiv/Aktiv-Modus aktivieren                   | Disabled                     |
   |                 |                   | Configure BGP (BGP konfigurieren)                               | Deaktiviert                     |
   | Überprüfen + erstellen |                   | Überprüfen Sie die Einstellungen, und wählen Sie **Erstellen** aus. |                              |

   > [!NOTE]
   >
   > Die Erstellung eines Gateways für das virtuelle Netzwerk kann bis zu 45 Minuten dauern.

## Aufgabe 8: Verbinden von CoreServicesVnet mit ManufacturingVnet

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie unter Gateways für virtuelle Netzwerke die Option **CoreServicesVnetGateway** aus.

1. Wählen Sie in CoreServicesGateway die Option **Verbindungen** und dann **+ Hinzufügen** aus.

   > [!NOTE]
   >
   >  Sie können diese Konfiguration erst abschließen, wenn die Gateways für virtuelle Netzwerke vollständig bereitgestellt wurden.

1. Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen der Verbindung:

   | **Option**                     | **Wert**                         |
   | ------------------------------ | --------------------------------- |
   | Name                           | CoreServicesGW-to-ManufacturingGW |
   | Verbindungstyp                | VNet-zu-VNet                      |
   | Erstes Gateway für virtuelle Netzwerke  | CoreServicesVnetGateway           |
   | Zweites Gateway für virtuelle Netzwerke | ManufacturingVnetGateway          |
   | Gemeinsam verwendeter Schlüssel (PSK)               | abc123                            |
   | Private Azure-IP-Adresse verwenden   | Nicht ausgewählt                      |
   | BGP aktivieren                     | Nicht ausgewählt                      |
   | IKE-Protokoll                   | IKEv2                             |
   | Subscription                   | Keine Änderungen erforderlich               |
   | Resource group                 | Keine Änderungen erforderlich               |
   | Standort                       | USA, Osten                           |

1. Wählen Sie **OK** aus, um die Verbindung zu erstellen.

## Aufgabe 9: Verbinden von ManufacturingVnet mit CoreServicesVnet

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie unter Gateways für virtuelle Netzwerke die **Option ManufacturingVnetGateway** aus.

1. Wählen Sie in CoreServicesGateway die Option **Verbindungen** und dann **+ Hinzufügen** aus.

1. Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen der Verbindung:

   | **Option**                     | **Wert**                         |
   | ------------------------------ | --------------------------------- |
   | Name                           | ManufacturingGW-to-CoreServicesGW |
   | Verbindungstyp                | VNet-zu-VNet                      |
   | Erstes Gateway für virtuelle Netzwerke  | ManufacturingVnetGateway          |
   | Zweites Gateway für virtuelle Netzwerke | CoreServicesVnetGateway           |
   | Gemeinsam verwendeter Schlüssel (PSK)               | abc123                            |
   | Private Azure-IP-Adresse verwenden   | Nicht ausgewählt                      |
   | BGP aktivieren                     | Nicht ausgewählt                      |
   | IKE-Protokoll                   | IKEv2                             |
   | Subscription                   | Keine Änderungen erforderlich               |
   | Resource group                 | Keine Änderungen erforderlich               |
   | Standort                       | Europa, Westen                       |

1. Wählen Sie **OK** aus, um die Verbindung zu erstellen.

## Aufgabe 10: Überprüfen, ob die Verbindungen stehen

1. Geben Sie unter **Ressourcen, Dienste und Dokumente suchen (G+/)** **VPN** ein und wählen Sie dann **Verbindungen** aus den Ergebnissen aus.

1. Warten Sie, bis der Status beider Verbindungen auf **Verbunden** steht. Möglicherweise müssen Sie Ihren Bildschirm aktualisieren.

   ![VPN Gateway-Verbindungen erfolgreich erstellt.](../media/connections-status-connected.png)

## Aufgabe 11: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf **ManufacturingVM** PowerShell.

1. Prüfen Sie mit dem folgenden Befehl, dass jetzt eine Verbindung zu CoreServicesVM auf CoreServicesVnet besteht. Achten Sie darauf, die IPv4-Adresse für CoreServicesVM zu verwenden.

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

1. Die Testverbindung sollte erfolgreich erstellt werden, und es wird ein ähnliches Ergebnis wie das folgende zu sehen sein:

   ![Test-NetConnection erfolgreich.](../media/test-connection-succeeded.png)

1. Schließen Sie die Fenster der Remotedesktopverbindung.

Herzlichen Glückwunsch! Sie haben eine VNet-zu-VNet-Verbindung mithilfe eines Gateways für virtuelle Netzwerke konfiguriert.
