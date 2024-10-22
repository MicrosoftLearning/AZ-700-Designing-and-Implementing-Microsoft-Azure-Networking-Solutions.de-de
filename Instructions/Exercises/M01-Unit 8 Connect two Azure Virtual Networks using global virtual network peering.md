---
Exercise:
  title: "M01 – Lerneinheit\_8: Verbinden von zwei virtuellen Azure-Netzwerken mithilfe des globalen Peerings virtueller Netzwerke"
  module: Module 01 - Introduction to Azure Virtual Networks
---

# M01 – Lerneinheit 8: Verbinden von zwei virtuellen Azure-Netzwerken mithilfe des globalen Peerings virtueller Netzwerke

## Übungsszenario

In dieser Lerneinheit konfigurieren Sie die Konnektivität zwischen „CoreServicesVnet“ und „ManufacturingVnet“, indem Sie Peerings zum Zulassen von Datenverkehr hinzufügen.

![Abbildung: Peering virtueller Netzwerke.](../media/8-exercise-connect-two-azure-virtual-networks-global.png)

Inhalt dieser Einheit:

+ Aufgabe 1: Erstellen einer virtuellen Maschine zum Testen der Konfiguration
+ Aufgabe 2: Herstellen einer Verbindung mit den Test-VMs mit RDP
+ Aufgabe 3: Testen der Verbindung zwischen den VMs
+ Aufgabe 4: Erstellen von VNet-Peerings zwischen CoreServicesVnet und ManufacturingVnet
+ Aufgabe 5: Testen der Verbindung zwischen den VMs
+ Aufgabe 6: Bereinigen der Ressourcen

**Hinweis:** Eine **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering)** ist verfügbar, mit der Sie dieses Lab in Ihrem eigenen Tempo durcharbeiten können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und dem gehosteten Lab vor, aber die dargestellten Kernkonzepte und Ideen sind identisch.

### Geschätzte Dauer: 20 Minuten

## Aufgabe 1: Erstellen einer virtuellen Maschine zum Testen der Konfiguration

In diesem Abschnitt erstellen Sie eine Test-VM im VNet für die Fertigung, um zu testen, ob Sie über „ManufacturingVnet“ auf Ressourcen in einem anderen virtuellen Azure-Netzwerk zugreifen können.

### Erstellen von „ManufacturingVM“

1. Wählen Sie im Azure-Portal das Cloud Shell-Symbol (oben rechts). Konfigurieren Sie die Shell bei Bedarf.  
    + Wählen Sie **PowerShell** aus.
    + Wählen Sie **Kein Speicherkonto erforderlich** und Ihr **Abonnement** aus und klicken Sie dann auf **Anwenden**.
    + Warten Sie, bis das Terminal erstellt wurde und eine Eingabeaufforderung angezeigt wird. 

1. Wählen Sie in der Symbolleiste des Bereichs „Cloud Shell" das Symbol **Dateien verwalten**, wählen Sie im Dropdown-Menü **Hochladen** und laden Sie die folgenden Dateien **ManufacturingVMazuredeploy.json** und **ManufacturingVMazuredeploy.parameters.json** in das Startverzeichnis der Cloud Shell aus dem Quellordner **F:\Allfiles\Exercises\M01** hoch.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderlichen VMs zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Überprüfen Sie, ob der virtuelle Computer erstellt wurde.

## Aufgabe 2: Herstellen einer Verbindung mit den Test-VMs mit RDP

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.

1. Wählen Sie **ManufacturingVM** aus.

1. Wählen Sie unter „ManufacturingVM“ die Optionen **Verbinden &gt; RDP** aus.

1. Wählen Sie unter „ManufacturingVM \| Verbinden“ die Option **RDP-Datei herunterladen** aus.

1. Speichern Sie die RDP-Datei auf Ihrem Desktop.

1. Stellen Sie eine Verbindung zu ManufacturingVM her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, das Sie während der Bereitstellung angegeben haben.

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.

1. Wählen Sie **TestVM1** aus.

1. Wählen Sie unter TestVM1 **Verbinden &gt; RDP** aus.

1. Wählen Sie unter „TestVM1 \| Verbinden“ die Option **RDP-Datei herunterladen** aus.

1. Speichern Sie die RDP-Datei auf Ihrem Desktop.

1. Stellen Sie eine Verbindung zu TestVM1 her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, die Sie bei der Bereitstellung angegeben haben.

1. Wählen Sie für beide VMs unter **Wählen Sie die Datenschutzeinstellungen für Ihr Gerät aus** die Option **Akzeptieren** aus.

1. Wählen Sie für beide VMs unter **Netzwerke** die Option **Ja** aus.

1. Öffnen Sie auf TestVM1 eine PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus: ipconfig

1. Notieren Sie die IPv4-Adresse.

## Aufgabe 3: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf der ManufacturingVM eine PowerShell-Eingabeaufforderung.

1. Stellen Sie mithilfe des folgenden Befehls sicher, dass keine Verbindung mit TestVM1 im CoreServicesVnet besteht. Achten Sie darauf, die IPv4-Adresse für TestVM1 zu verwenden.

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. Die Testverbindung sollte fehlschlagen, und es wird ein ähnliches Ergebnis wie das folgende angezeigt: ![PowerShell-Fenster mit Fehleranzeige für Befehl „Test-NetConnection 10.20.20.4 -port 3389“](../media/test-netconnection-fail.png)

## Aufgabe 4: Erstellen von VNet-Peerings zwischen CoreServicesVnet und ManufacturingVnet

1. Wählen Sie auf der Azure-Startseite **Virtuelle Netzwerke** und anschließend **CoreServicesVnet** aus.

1. Wählen Sie in „CoreServicesVnet“ unter **Einstellungen** die Option **Peerings** aus.
   ![Screenshot der Kerndienste der VNet-Peering-Einstellungen ](../media/create-peering-on-coreservicesvnet.png)

1. Wählen Sie in „CoreServicesVnet \| Peerings“ die Option **+ Hinzufügen** aus.

1. Verwenden Sie diese Informationen, um das Peering zu erstellen. Wählen Sie abschließend **Hinzufügen** aus. 

   **Zusammenfassung zu virtuellen Remotenetzwerken**

   | **Option**                                    | **Wert**                             |
   | ------------------------------------ | --------------------------------------------- | 
   | Name des Peeringlinks    | `CoreServicesVnet-to-ManufacturingVnet` |
   | Virtuelles Netzwerk | ManufacturingVnet |

    **Einstellungen für das Peering virtueller Remotenetzwerke**
   
   | **Option**                                    | **Wert**                             |
   | ------------------------------------ | --------------------------------------------- | 
   | Erlauben Sie 'ManufacturingVnet' den Zugriff auf 'CoreServicesVnet' | Aktiviert |
   |ManufacturingVnet“ soll weitergeleiteten Verkehr von ‚CoreServicesVnet‘ empfangen | Aktiviert |
 
    **Lokales virtuelles Netzwerk: Zusammenfassung**

    | **Option**                                    | **Wert**                             |
    | ------------------------------------ | --------------------------------------------- | 
    | Name des Peeringlinks | `CoreServicesVnet-to-ManufacturingVnet` |
 
    **Einstellungen für das Peering virtueller Remotenetzwerke**
   
    | **Option**                                    | **Wert**                             |
    | ------------------------------------ | --------------------------------------------- | 
    | Erlauben Sie 'CoreServicesVnet' den Zugriff auf 'ManufacturingVnet' | Aktiviert
    | Erlauben Sie, dass 'CoreServicesVnet' weitergeleiteten Verkehr von 'ManufacturingVnet' empfängt | Aktiviert |
 
1. Überprüfen Sie in „CoreServicesVnet" \| Peerings, dass das **CoreServicesVnet-to-ManufacturingVnet** Peering **Verbunden** ist.

1. Wählen Sie unter „Virtuelle Netzwerke" die Option **ManufacturingVnet**, und überprüfen Sie, ob das Peering **ManufacturingVnet-to-CoreServicesVnet** **Verbunden** ist.

## Aufgabe 5: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf der ManufacturingVM eine PowerShell-Eingabeaufforderung.

1. Stellen Sie mithilfe des folgenden Befehls sicher, dass jetzt eine Verbindung mit TestVM1 im CoreServicesVnet besteht.

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. Die Testverbindung sollte erfolgreich sein, und es wird ein ähnliches Ergebnis wie das folgende angezeigt: ![PowerShell-Fenster mit Erfolgsmeldung „TcpTestSucceeded: True“ für Befehl „Test-NetConnection 10.20.20.4 -port 3389“](../media/test-connection-succeeded.png)

Herzlichen Glückwunsch! Sie haben die Konnektivität zwischen den VNets erfolgreich konfiguriert, indem Sie Peerings hinzugefügt haben.

## Aufgabe 6: Bereinigen der Ressourcen

   >**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Gebühren anfallen.

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung. (Erstellen Sie bei Bedarf Cloud Shell-Speicher mithilfe der Standardeinstellungen.)

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   ```

   >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.
