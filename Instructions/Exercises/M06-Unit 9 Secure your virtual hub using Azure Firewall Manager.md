---
Exercise:
  title: "M06 – Lerneinheit\_9: Schützen Ihres virtuellen Hubs mit Azure Firewall Manager"
  module: Module 06 - Design and implement network security
---


# M06 – Lerneinheit 9: Schützen Ihres virtuellen Hubs mit Azure Firewall Manager

## Übungsszenario

In dieser Übung erstellen Sie das virtuelle Spoke-Netzwerk und einen geschützten virtuellen Hub. Anschließend verbinden Sie die virtuellen Hub-and-Spoke-Netzwerke und routen Datenverkehr an Ihren Hub. Im nächsten Schritt stellen Sie die Workloadserver bereit und erstellen dann eine Firewallrichtlinie und schützen Ihren Hub. Schließlich testen Sie die Firewall.

![Abbildung: Architektur des virtuellen Netzwerks mit einem sicheren Hub](../media/9-exercise-secure-your-virtual-hub-using-azure-firewall-manager.png)


   >**Hinweis:** Eine **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Secure%20your%20virtual%20hub%20using%20Azure%20Firewall%20Manager)** ist verfügbar, mit der Sie dieses Lab in Ihrem eigenen Tempo durcharbeiten können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und dem gehosteten Lab vor, aber die dargestellten Kernkonzepte und Ideen sind identisch.

## Erstellen einer Hub-Spoke-Architektur

In diesem Teil der Übung erstellen Sie die virtuellen Spoke-Netzwerke und Subnetze, in denen Sie die Workloadserver platzieren. Anschließend erstellen Sie den geschützten virtuellen Hub und verbinden die virtuellen Hub-and-Spoke-Netzwerke .

In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen von zwei virtuellen Spoke-Netzwerken und Subnetzen
+ Aufgabe 2: Erstellen des geschützten virtuellen Hubs
+ Aufgabe 3: Verbinden der virtuellen Hub-and-Spoke-Netzwerke
+ Aufgabe 4: Bereitstellen der Server
+ Aufgabe 5: Erstellen einer Firewallrichtlinie und Schützen Ihres Hubs
+ Aufgabe 6: Zuordnen der Firewallrichtlinie
+ Aufgabe 7: Weiterleiten von Datenverkehr an Ihren Hub
+ Aufgabe 8: Testen der Anwendungsregel
+ Aufgabe 9: Testen der Netzwerkregel
+ Aufgabe 10: Bereinigen von Ressourcen

### Geschätzte Dauer: 35 Minuten

## Aufgabe 1: Erstellen von zwei virtuellen Spoke-Netzwerken und Subnetzen

In dieser Aufgabe erstellen Sie die beiden virtuellen Spoke-Netzwerke, die jeweils ein Subnetz enthalten, das Ihre Workloadserver hostet.

1. Geben Sie auf der Startseite des Azure-Portals in das Suchfeld **Virtuelles Netzwerk** ein, und wählen Sie **Virtuelles Netzwerk**, wenn diese Option angezeigt wird.

1. Klicken Sie auf **Erstellen**.

1. Wählen Sie unter **Ressourcengruppe** die Option **Neue erstellen** aus, geben Sie **fw-manager-rg** als Namen ein, und wählen Sie dann **OK** aus.

1. Geben unter **Name** den Namen **Spoke-01** ein.

1. Wählen Sie unter **Region** Ihre Region aus.

1. Klicken Sie auf **Weiter: IP-Adressen**.

1. Geben Sie unter **IPv4-Adressraum** die Angabe **10.0.0.0/16** ein.

1. **Löschen** Sie alle anderen hier aufgeführten Adressräume, **z. B. 10.1.0.0/16**.

1. Wählen Sie unter **Subnetzname** das Wort **Standard** aus.

1. Ändern Sie im Dialogfeld **Subnetz bearbeiten** den Namen in **Workload-01-SN**.

1. Ändern Sie den **Subnetzadressbereich** in **10.0.1.0/24**.

1. Wählen Sie **Speichern**.

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Klicken Sie auf **Erstellen**.

Wiederholen Sie die Schritte 1 bis 14 oben, um ein weiteres ähnliches virtuelles Netzwerk und Subnetz zu erstellen, verwenden Sie aber die folgenden Informationen:

+ Ressourcengruppe: **fw-manager-rg** (vorhandene auswählen)
+ Name: **Spoke-02**
+ Adressraum: **10.1.0.0/16** (löschen Sie alle anderen aufgelisteten Adressräume)
+ Subnetzname: **Workload-02-SN**
+ Subnetzadressbereich: **10.1.1.0/24**

## Aufgabe 2: Erstellen des geschützten virtuellen Hubs

In dieser Aufgabe erstellen Sie Ihren geschützten virtuellen Hub mithilfe von Firewall Manager.

1. Wählen Sie auf der Startseite des Azure-Portals **Alle Dienste** aus.

1. Geben Sie im Suchfeld **firewall manager** ein, und wählen Sie **Firewall Manager** aus, wenn diese Option angezeigt wird.

1. Wählen Sie auf der Seite **Firewall Manager** auf der Übersichtsseite die Option **Geschützte virtuelle Hubs anzeigen**.

1. Wählen Sie auf der Seite **Virtuelle Hubs** die Option **Neuen geschützten virtuellen Hub erstellen** aus.

1. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **fw-manager-rg** aus.

1. Wählen Sie als **Region** Ihre Region aus.

1. Geben Sie unter **Name des geschützten virtuellen Hubs** den Namen **Hub-01** ein.

1. Geben Sie unter **Hubadressraum** die Angabe **10.2.0.0/16** ein.

1. Wählen Sie **Neues vWAN** aus.

1. Geben Sie unter **Virtual WAN-Name** den Namen **Vwan-01** ein.

1. Klicken Sie auf **Weiter: Azure Firewall** aus.
    ![Erstellen eines neuen geschützten virtuellen Hubs: Registerkarte „Grundlagen“](../media/create-new-secured-virtual-hub-1.png)

1. Klicken Sie auf **Weiter: Sicherheitspartneranbieter**.

1. Klicken Sie auf **Weiter: Überprüfen + erstellen**.

1. Klicken Sie auf **Erstellen**.

    > **[!NOTE]**
    >
    > Der Vorgang der Bereitstellung kann bis zu 30 Minuten in Anspruch nehmen.

    

    ![Erstellen eines neuen geschützten virtuellen Hubs: Registerkarte „Überprüfen und erstellen“](../media/create-new-secured-virtual-hub-2.png)

1. Wählen Sie nach Abschluss der Bereitstellung auf der Homepage des Azure-Portals **Alle Dienste** aus.

1. Geben Sie im Suchfeld **firewall manager** ein, und wählen Sie **Firewall Manager** aus, wenn diese Option angezeigt wird.

1. Wählen Sie auf der Seite **Firewall Manager** die Option **Virtuelle Hubs** aus.

1. Wählen Sie **Hub-01** aus.

1. Wählen Sie **Öffentliche IP-Konfiguration** aus.

1. Notieren Sie sich die öffentliche IP-Adresse (z. B. **51.143.226.18**), die Sie später verwenden.

## Aufgabe 3: Verbinden der virtuellen Hub-and-Spoke-Netzwerke

In dieser Aufgabe verbinden Sie die virtuellen Hub-and-Spoke-Netzwerke. Dies wird häufig als „Peering“ bezeichnet.

1. Wählen Sie auf der Homepage des Azure-Portals **Ressourcengruppen** aus.

1. Wählen Sie die Ressourcengruppe **fw-manager-rg** und anschließend das virtuelle WAN **Vwan-01** aus.

1. Wählen Sie unter **Konnektivität** die Option **Virtuelle Netzwerkverbindungen** aus.

1. Wählen Sie **Verbindung hinzufügen** aus.

1. Geben Sie unter **Verbindungsname** den Namen **hub-spoke-01** ein.

1. Wählen Sie unter **Hubs** die Option **Hub-01** aus.

1. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **fw-manager-rg** aus.

1. Wählen Sie unter **Virtuelles Netzwerk** die Option **Spoke-01** aus.

1. Klicken Sie auf **Erstellen**.
   ![Hinzufügen einer Hub-and-Spoke-Verbindung mit dem virtuellen WAN: Spoke 1](../media/connect-hub-spoke-vnet-1.png)

1. Wiederholen Sie die oben genannten Schritte 4 bis 9, um eine weitere ähnliche Verbindung zu erstellen, verwenden Sie aber den Verbindungsnamen **hub-spoke-02**, um das virtuelle Netzwerk **Spoke-02** zu verbinden.

    ![Hinzufügen einer Hub-and-Spoke-Verbindung mit dem virtuellen WAN: Spoke 2](../media/connect-hub-spoke-vnet-2.png)

## Aufgabe 4: Bereitstellen der Server

1. Wählen Sie im Azure-Portal das Cloud Shell-Symbol (oben rechts). Konfigurieren Sie die Shell bei Bedarf.  
    + Wählen Sie **PowerShell** aus.
    + Wählen Sie **Kein Speicherkonto erforderlich** und Ihr **Abonnement** aus und klicken Sie dann auf **Anwenden**.
    + Warten Sie, bis das Terminal erstellt wurde und eine Eingabeaufforderung angezeigt wird. 

1. Wählen Sie in der Symbolleiste des Cloud Shell-Fensters das Symbol **Dateien verwalten** aus, wählen Sie im Dropdownmenü die Option **Hochladen** und laden Sie die folgenden Dateien **FirewallManager.json** und **FirewallManager.parameters.json** in das Cloud Shell-Basisverzeichnis hoch.

    > **Hinweis:** Wenn Sie in Ihrem eigenen Abonnement arbeiten, sind die [Vorlagendateien](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/tree/master/Allfiles/Exercises) im GitHub-Lab-Repository verfügbar.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderliche VM zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "fw-manager-rg"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile FirewallManager.json -TemplateParameterFile FirewallManager.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Notieren Sie sich auf der Seite **Übersicht** von **Srv-workload-01** im rechten Bereich unter dem Abschnitt **Netzwerk** die **Private IP-Adresse** (z. B. **10.0.1.4**).

1. Notieren Sie sich auf der Seite **Übersicht** von **Srv-workload-02** im rechten Bereich unter dem Abschnitt **Netzwerk** die **Private IP-Adresse** (z. B. **10.1.1.4**).

## Aufgabe 5: Erstellen einer Firewallrichtlinie und Schützen Ihres Hubs

In dieser Aufgabe erstellen Sie zuerst Ihre Firewallrichtlinie und schützen dann Ihren Hub. Die Firewallrichtlinie definiert Regelsammlungen für die Weiterleitung von Datenverkehr an mindestens einen geschützten virtuellen Hub.

1. Wählen Sie auf der Homepage des Azure-Portals **Firewall Manager** aus.
   + Wenn das Firewall Manager-Symbol nicht auf der Homepage angezeigt wird, wählen Sie **Alle Dienste** aus. Geben Sie dann im Suchfeld **firewall manager** ein, und wählen Sie **Firewall Manager** aus, wenn diese Option angezeigt wird.

1. Wählen Sie in **Firewall Manager** auf der Übersichtsseite die Option **Azure Firewall-Richtlinien anzeigen** aus.

1. Wählen Sie die Option **Azure-Firewallrichtlinie erstellen** aus.

1. Wählen Sie unter **Ressourcengruppe** die Option **fw-manager-rg** aus.

5. Geben Sie unter **Richtliniendetails** unter **Name** die Angabe **Policy-01** ein.

1. Wählen Sie unter **Region** Ihre Region aus.

1. Wählen Sie **Standard** als **Richtlinienebene** aus.

1. Wählen Sie **Weiter: DNS-Einstellungen** aus.

1. Wählen Sie **Weiter: TLS-Überprüfung (Vorschau)** aus.

1. Wählen Sie **Weiter: Regeln** aus.

1. Wählen Sie auf der Registerkarte **Regeln** die Option **Regelsammlung hinzufügen** aus.

1. Geben Sie auf der Seite **Regelsammlung hinzufügen** unter **Name** die Angabe **App-RC-01** ein.

1. Wählen Sie unter **Regelsammlungstyp** die Option **Anwendung** aus.

1. Geben Sie unter **Priorität**den Wert **100** ein.

1. Vergewissern Sie sich, dass die **Regelsammlungsaktion** auf **Zulassen** festgelegt ist.

1. Geben Sie unter **Regeln** unter **Name** den Namen **Allow-msft** ein.

1. Wählen Sie unter **Quellentyp** die Option **IP-Adresse** aus.

1. Geben Sie unter **Quelle** „*“ ein.

1. Geben Sie unter **Protokoll** die Angabe **http,https** ein.

1. Vergewissern Sie sich, dass **Zieltyp** auf **FQDN** festgelegt ist.

1. Geben Sie unter **Ziel** die Zeichenfolge ***.microsoft.com** ein.

1. Wählen Sie **Hinzufügen** aus.

    ![Hinzufügen der Anwendungsregelsammlung zur Firewallrichtlinie](../media/add-rule-collection-firewall-policy-1.png)

1. Um eine DNAT-Regel hinzuzufügen, damit Sie einen Remotedesktop mit der VM „Srv-workload-01“ verbinden können, wählen Sie **Regelsammlung hinzufügen** aus.

1. Geben Sie unter **Name** den Namen **dnat-rdp** ein.

1. Wählen Sie unter **Regelsammlungstyp** die Option **DNAT** aus.

1. Geben Sie unter **Priorität**den Wert **100** ein.

1. Geben Sie unter **Regeln** unter **Name** den Namen **Allow-rdp** ein.

1. Wählen Sie unter **Quellentyp** die Option **IP-Adresse** aus.

1. Geben Sie unter **Quelle** „*“ ein.

1. Wählen Sie für **Protokoll** die Option **TCP** aus.

1. Geben Sie unter **Zielports** den Wert **3389** ein.

1. Wählen Sie unter **Zieltyp** die Option **IP-Adresse** aus.

1. Geben Sie unter **Ziel** die öffentliche IP-Adresse des virtuellen Firewallhubs ein, die Sie sich zuvor notiert haben (z. B. **51.143.226.18**).

1. Geben Sie unter **Übersetzte Adresse** die private IP-Adresse für **Srv-workload-01** ein, die Sie sich zuvor notiert haben (z. B. **10.0.1.4**).

1. Geben Sie unter **Übersetzter Port** den Wert **3389** ein.

1. Wählen Sie **Hinzufügen** aus.

1. Um eine Netzwerkregel hinzuzufügen, damit Sie einen Remotedesktop von der VM „Srv-workload-01“ mit der VM „Srv-workload-02“ verbinden können, wählen Sie **Regelsammlung hinzufügen** aus.

1. Geben Sie unter **Name** den Namen **vnet-rdp** ein.

1. Wählen Sie unter **Regelsammlungstyp** die Option **Netzwerk** aus.

1. Geben Sie unter **Priorität**den Wert **100** ein.

1. Wählen Sie unter **Regelsammlungsaktion** die Option **Zulassen** aus.

1. Geben Sie unter **Regeln** unter **Name** den Namen **Allow-vnet** ein.

1. Wählen Sie unter **Quellentyp** die Option **IP-Adresse** aus.

1. Geben Sie unter **Quelle** „*“ ein.

1. Wählen Sie für **Protokoll** die Option **TCP** aus.

1. Geben Sie unter **Zielports** den Wert **3389** ein.

1. Wählen Sie unter **Zieltyp** die Option **IP-Adresse** aus.

1. Geben Sie unter **Ziel** die private IP-Adresse für **Srv-workload-02** ein, die Sie sich zuvor notiert haben (z. B. **10.1.1.4**).

1. Wählen Sie **Hinzufügen** aus.

    ![Auflisten von Regelsammlungen in der Firewallrichtlinie](../media/list-rule-collections-firewall-policy.png)

1. Sie sollten nun drei Regelsammlungen aufgelistet werden.

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Klicken Sie auf **Erstellen**.

## Aufgabe 6: Zuordnen der Firewallrichtlinie

In dieser Aufgabe ordnen Sie die Firewallrichtlinie dem virtuellen Hub zu.

1. Wählen Sie auf der Homepage des Azure-Portals **Firewall Manager** aus.
   + Wenn das Firewall Manager-Symbol nicht auf der Homepage angezeigt wird, wählen Sie **Alle Dienste** aus. Geben Sie dann im Suchfeld **firewall manager** ein, und wählen Sie **Firewall Manager** aus, wenn diese Option angezeigt wird.

1. Wählen Sie in **Firewall Manager** unter **Sicherheit** die Option **Azure Firewall-Richtlinien** aus.

1. Aktivieren Sie das Kontrollkästchen für **Policy-01**.

1. Wählen Sie **Zuordnungen verwalten&gt;Hubs zuordnen** aus.

1. Aktivieren Sie das Kontrollkästchen für **Hub-01**.

1. Wählen Sie **Hinzufügen** aus.

1. Wenn die Richtlinie angefügt wurde, wählen Sie **Aktualisieren** aus. Die Zuordnung sollte angezeigt werden.

![Anzeigen der zugeordneten Firewallrichtlinie für den Hub](../media/associate-firewall-policy-with-hub-end.png)

## Aufgabe 7: Weiterleiten von Datenverkehr an Ihren Hub

In dieser Aufgabe stellen Sie sicher, dass Netzwerkdatenverkehr durch Ihre Firewall weitergeleitet wird.

1. Wählen Sie in **Firewall Manager** die Option **Virtuelle Hubs** aus.
1. Wählen Sie **Hub-01** aus.
1. Wählen Sie unter **Einstellungen** die Option **Sicherheitskonfiguration** aus.
1. Wählen Sie unter **Internetdatenverkehr** die Option **Azure Firewall** aus.
1. Wählen Sie unter **Privater Datenverkehr** die Option **Über Azure Firewall senden** aus.
1. Wählen Sie **Speichern**.
1. Der Erstellungsvorgang dauert ein paar Minuten.
1. Stellen Sie nach Abschluss der Konfiguration sicher, dass unter **INTERNETDATENVERKEHR** und **PRIVATER DATENVERKEHR** für beide Hub-Spoke-Verbindungen die Angabe **Geschützt durch Azure Firewall** angezeigt wird.

## Aufgabe 8: Testen der Anwendungsregel

In diesem Teil der Übung verbinden Sie einen Remotedesktop mit der öffentlichen IP-Adresse der Firewall, die mit Netzwerkadressenübersetzung in „Srv-Workload-01“ aufgelöst wird. Anschließend verwenden Sie einen Browser, um die Anwendungsregel zu testen, und verbinden einen Remotedesktop mit Srv-Workload-02, um die Netzwerkregel zu testen.

In dieser Aufgabe testen Sie die Anwendungsregel, um zu bestätigen, dass sie wie erwartet funktioniert.

1. Öffnen Sie **Remotedesktopverbindung** auf Ihrem PC.

1. Geben Sie im Feld **Computer** die **öffentliche IP-Adresse der Firewall** ein (z. B. **51.143.226.18**).

1. Wählen Sie **Optionen anzeigen** aus.

1. Geben Sie im Feld **Benutzername** den Namen **TestUser** ein.

1. Wählen Sie **Verbinden** aus.

   ![RDP-Verbindung mit srv-workload-01](../media/rdp-srv-workload-01.png)

1. Melden Sie sich im Dialogfeld **Anmeldeinformationen eingeben** beim virtuellen Servercomputer **Srv-workload-01** an, indem Sie das Kennwort verwenden, das Sie bei der Bereitstellung angegeben haben.

1. Klickan Sie auf **OK**.

1. Wählen Sie in der Zertifikatmeldung **Ja** aus.

1. Öffnen Sie Internet Explorer, und wählen Sie im Dialogfeld **Internet Explorer 11 einrichten** die Option **OK** aus.

1. Wechseln Sie zu **https://** **<www.microsoft.com>**.

1. Wählen Sie im Dialogfeld **Sicherheitswarnung** die Option **OK** aus.

1. Wählen Sie in den Internet Explorer-Sicherheitswarnungen, die möglicherweise angezeigt werden, **Schließen** aus.

1. Daraufhin sollte die Microsoft-Homepage angezeigt werden.

    ![RDP-Sitzung zeigt microsoft.com an](../media/microsoft-home-page.png)

1. Wechseln Sie zu **https://** **<www.google.com>**.

1. Sie sollten durch die Firewall blockiert werden.

    ![RDP-Sitzungsbrowser für google.com blockiert](../media/google-home-page-fail.png)

1. Sie haben also bestätigt, dass Sie eine Verbindung mit einem zulässigen FQDN herstellen können, aber für alle anderen blockiert werden.

## Aufgabe 9: Testen der Netzwerkregel

In dieser Aufgabe testen Sie die Netzwerkregel, um zu bestätigen, dass sie wie erwartet funktioniert.

1. Öffnen Sie auf diesem Remotecomputer **Remotedesktopverbindung**, während Sie noch bei der RDP-Sitzung **Srv-workload-01** angemeldet sind.

1. Geben Sie im Feld **Computer** die **private IP-Adresse** von **Srv-workload-02** ein (z. B. **10.1.1.4**).

1. Melden Sie sich im Dialogfeld **Anmeldeinformationen eingeben** auf dem Server **Srv-workload-02** mit dem Benutzernamen **TestUser** und dem Kennwort an, das Sie bei der Bereitstellung angegeben haben.

1. Klickan Sie auf **OK**.

1. Wählen Sie in der Zertifikatmeldung **Ja** aus.

   ![RDP-Sitzung von srv-workload-01 zu einer anderen RDP-Sitzung auf srv-workload-02](../media/rdp-srv-workload-02-from-srv-workload-01.png)

1. Sie haben nun also bestätigt, dass die Firewallnetzwerkregel funktioniert, da Sie einen Remotedesktop von einem Server mit einem anderen Server in einem anderen virtuellen Netzwerk verbunden haben.

1. Schließen Sie beide RDP-Sitzungen, um sie zu trennen.

## Aufgabe 10: Bereinigen von Ressourcen

>**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Gebühren anfallen.

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Remove-AzResourceGroup -Name 'fw-manager-rg' -Force -AsJob
   ```

   >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.
