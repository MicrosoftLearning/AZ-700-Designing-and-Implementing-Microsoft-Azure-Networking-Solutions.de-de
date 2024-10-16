---
demo:
  module: Module 01 - Introduction to Azure Virtual Networks
  title: Benutzerdefinierte Routen (Modul 01)
---
## Konfigurieren von Netzwerkrouting und Endpunkten

In dieser Demo erhalten wir Informationen zum Erstellen einer Routingtabelle, zum Definieren einer benutzerdefinierten Route sowie zum Zuordnen der Route zu einem Subnetz.

**Hinweis:**  Für diese Demo ist ein virtuelles Netzwerk mit mindestens einem Subnetz erforderlich.

**Referenz:** [Weiterleiten von Netzwerkdatenverkehr: Tutorial – Azure-Portal](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**Erstellen Sie eine Routingtabelle**

1. Wenn Sie Zeit haben, sehen Sie sich das Tutorialdiagramm an. Erläutern Sie, warum Sie eine benutzerdefinierte Route erstellen müssen. 

1. Öffnen Sie das Azure-Portal.

1. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus. Besprechen Sie, wann **verteilte Gatewayrouten** verwendet werden sollen. 

1. Erstellen Sie eine Routingtabelle, und erläutern Sie alle ungewöhnlichen Einstellungen. 

1. Warten Sie, bis die neue Routingtabelle bereitgestellt wurde.

**Hinzufügen einer Route**

1.  Wählen Sie Ihre neue Routingtabelle und dann  **Routen** aus.

1.  Erstellen Sie eine neue **Route**. Diskutieren Sie die verschiedenen **verfügbaren Hoptypen**. 

1.  Erstellen Sie die neue Route, und warten Sie, bis die Ressource bereitgestellt wird.
 
**Zuordnen einer Routingtabelle zu einem Subnetz**

1.  Navigieren Sie zu dem Subnetz, das Sie der Routingtabelle zuordnen möchten.

1.  Wählen Sie **Routingtabelle** aus, und wählen Sie Ihre neue Routingtabelle aus. 

1.  **Speichern** Sie Ihre Änderungen.


