Über das Projekt
----------------

Das Projekt "Digital Editions Footprint" untersucht den ökologischen Fußabdruck von digitalen Editionen in den Geisteswissenschaften. Es wird von der Digital Humanities Craft OG durchgeführt und ist Teil des Forschungsschwerpunkts "Nachhaltigkeit in den Digital Humanities". Das Projekt untersucht die Umweltauswirkungen von digitalen Editionen und entwickelt Strategien und Werkzeuge, um diese zu reduzieren. Ziel ist es, die Nachhaltigkeit von digitalen Editionen zu verbessern und Forschende und Editionsprojekte zu unterstützen, umweltfreundlichere Praktiken zu etablieren.

Zielgruppe
----------

*   Bestehende Editionen, die Optimierungen vornehmen möchten.
*   Neue Editionen, die von Anfang an korrekt handeln möchten.

Digitale Editionen
------------------

Digitale Editionen sind elektronische Versionen von Texten, Dokumenten oder literarischen Werken, die mithilfe digitaler Technologien erstellt, bearbeitet und publiziert werden. Sie nutzen das Potenzial des Internets und digitaler Speichermedien, um Zugang zu und Interaktion mit Materialien zu bieten, die sonst schwer zugänglich wären. Anders als traditionelle, gedruckte Editionen können digitale Editionen vielfältige Zusatzinformationen wie Kommentare, Übersetzungen, multimediale Inhalte und interaktive Elemente integrieren. Sie ermöglichen es, verschiedene Revisionen und Versionen eines Textes nebeneinander zu betrachten und bieten oft Werkzeuge zur Textanalyse. Digitale Editionen sind ein wichtiges Werkzeug in den Geisteswissenschaften, insbesondere in der Literaturwissenschaft, Geschichte und Philologie, da sie die Forschung und Lehre durch erweiterte Zugänglichkeit und neue Analysemöglichkeiten bereichern.

Unser Ansatz
------------

Die Integration eines einfachen CO2-Rechners in digitale Editionen verkörpert für uns einen pragmatischen Ansatz zur Adressierung des ökologischen Fußabdrucks in der digitalen Welt. Indem es die CO2-Emissionen sichtbar macht, schärft das Tool das Bewusstsein für die Umweltauswirkungen digitaler Inhalte und motiviert zu einem verantwortungsbewussteren Umgang mit digitalen Ressourcen. Es dient als Basis für datengetriebene Entscheidungen, um Websites energieeffizienter zu gestalten und fördert damit aktiv umweltfreundliche Praktiken. Schlüssel ist unserer Meinung nach die einfache Einbindung des Tools in bestehende und vor allem auch zukünftigen Projekten. Durch den simplen copy/paste Ansatz bietet das Tool einen niederschwelligen Einstieg in die Reduzierung des digitalen Fußabdrucks und leistet somit einen Beitrag zum umweltbewussten Handeln im digitalen Zeitalter in den Digital Humanities.

Das "Tool"
----------

Wir haben minimalen JavaScript-Code entwickelt, um direkt auf jeder beliebigen HTML-Seite die Menge an CO2 anzuzeigen.


```javascript
// Importing the @tgwf/co2 library from jsDelivr CDN
import { co2, hosting, averageIntensity } from "https://cdn.jsdelivr.net/npm/@tgwf/co2@latest/dist/esm/index.js";

(async () => {
  // Initialize elements and library instances
  const container = document.getElementById("co2-calculator-wrapper");

  const swd = new co2({ model: "swd" });
  const oneByte = new co2({ model: "1byte" });

  // Calculate emissions
  const emissions_swd_per_byte = swd.perByte(1000000000, true); // per GB
  const emissions_1byte_per_byte = oneByte.perByte(1000000000, true); // per GB

  // Page size calculation in bytes and converting it to KB
  const page_size = document.documentElement.outerHTML.length;
  const page_size_in_kb = (page_size / 1000).toFixed(0); // Convert to KB and fix to 0 decimal places
  const page_size_in_mb = (page_size / 1e6).toFixed(2); // Convert to MB and fix to 2 decimal places
  const emissions_swd_page_size = swd.perByte(page_size, true);

  // Hosting check - https://developers.thegreenwebfoundation.org/co2js/tutorials/check-hosting/
  const domain = window.location.hostname;
  const hosting_domain = await hosting.check(domain);

  // Austria average intensity - Change Country with ISO Code - All countries are represented by their respective Alpha-3 ISO country code
  const { AUT } = averageIntensity.data;

  // Dynamic content creation
  function addContentBlock(contents) {
    const details = document.createElement("details");
    const summary = document.createElement("summary");
    summary.textContent = "CO2 Emissions Information for this page"; // Customize your summary text here
    details.appendChild(summary);

    contents.forEach((content) => {
      const element = document.createElement("p");
      element.innerHTML = content.html;
      element.id = content.id;
      details.appendChild(element);
    });

    container.appendChild(details);
  }

  // Example usage
  addContentBlock([
    {
      id: "co2_element",
      html: `CO2-Emissionen von 1GB sind: <b>${emissions_swd_per_byte}</b> gCO2 pro kWh (swd Modell)`,
    },
    {
      id: "co2_element2",
      html: `CO2-Emissionen von 1GB sind: <b>${emissions_1byte_per_byte}</b> gCO2 pro kWh (1byte Modell)`,
    },
    {
      id: "co2_page",
      html: `Diese Seite ist ${page_size} Bytes groß (das sind ca. ${page_size_in_kb} KB oder ${page_size_in_mb} MB), die Menge an CO2 beträgt: <b>${emissions_swd_page_size.toFixed(6)}</b> gCO2 pro kWh (swd Modell)`,
    },
    {
      id: "hosting",
      html: `Wird unsere '${domain}' auf grüner Energie gehostet? <b>${hosting_domain}</b>`,
    },
    {
      id: "average",
      html: `Die durchschnittlichen CO2-Emissionen in Österreich betragen <b>${AUT}</b> gCO2 pro kWh`,
    },
  ]);
})();
```

Dieser JavaScript-Code verwendet die [CO2.js-Bibliothek](https://developers.thegreenwebfoundation.org/co2js) (`@tgwf/co2`), um den CO2-Fußabdruck einer Webseite zu berechnen und zu prüfen, ob die Seite auf einem umweltfreundlichen Hosting (Green Hosting) basiert. Der Code läuft asynchron, was bedeutet, dass er Operationen ausführen kann, die auf externe Daten warten, ohne den Rest der Seite beim Laden zu blockieren.

Die Bibliothek CO2.js, entwickelt von [The Green Web Foundation](https://www.thegreenwebfoundation.org/) , bietet Werkzeuge zur Abschätzung des CO2-Ausstoßes von digitalen Diensten und Produkten, insbesondere Webseiten. Die Bibliothek basiert auf Forschung und Daten zu den CO2-Emissionen, die mit der Datenübertragung und -speicherung im Internet verbunden sind. Sie ermöglicht es Entwickler\*innen, die Umweltauswirkungen ihrer Projekte zu messen und Strategien zur Reduktion des Carbon Footprints zu entwickeln. Die Bibliothek stellt insbesondere zwei Modelle zur Verfügung, um den CO2-Ausstoß zu berechnen: das `1byte` (OneByte) Modell und das `swd` (Sustainable Web Design) Modell.

### OneByte Modell

Das `1byte` Modell nutzt eine direkte Umrechnung von übertragenen Datenmengen in CO2-Emissionen, fokussiert sich dabei auf die Energieverbrauchsphasen von Rechenzentren und Netzwerken. Es verwendet eng definierte Systemgrenzen und liefert ein einfaches, direktes Ergebnis basierend auf der Datenmenge.

### SWD (Sustainable Web Design) Modell

Das `swd` Modell, kurz für Sustainable Web Design, Bietet eine umfassende Berechnung, die die gesamte Energieverwendung von Datenzentren, Netzwerken und Nutzergeräten einbezieht sowie die Gesamtenergie, die zur Erstellung dieser Systeme benötigt wird. Es berücksichtigt vollständige Lebenszyklusemissionen, inklusive Produktion und Versorgung, und liefert detaillierte, granulare Emissionsschätzungen.

### Nutzung der Modelle im Code

Im bereitgestellten Codebeispiel werden Instanzen der `co2` Klasse mit beiden Modellen (`swd` und `1byte`) erstellt, um den CO2-Ausstoß pro Byte für 1GB Datenübertragung zu berechnen. Darüber hinaus wird das `swd` Modell verwendet, um die CO2-Emissionen der aktuellen Seite basierend auf ihrer Größe zu schätzen. Diese Ansätze ermöglichen eine differenzierte Analyse der Umweltauswirkungen einer Webseite, indem sowohl die durchschnittlichen globalen Emissionen als auch die spezifischen Bedingungen der Nutzung auf kleinen Webgeräten berücksichtigt werden.

### Import der Bibliothek

*   Der Code importiert zunächst die benötigten Funktionen (`co2`, `hosting`, `averageIntensity`) von der `@tgwf/co2` Bibliothek über das CDN (Content Delivery Network) jsDelivr. Dies ermöglicht es, die neueste Version der Bibliothek direkt im Browser zu verwenden, ohne sie manuell herunterladen zu müssen.

### Initialisierung und Berechnung

*   Im nächsten Schritt initialisiert der Code einige Konstanten und Instanzen der `co2` Klasse mit unterschiedlichen Modellen (`swd` und `1byte`), um später den CO2-Ausstoß pro Byte zu berechnen.
*   Es wird der CO2-Ausstoß für 1 Gigabyte (1GB) Daten berechnet, sowohl für das `swd` als auch für das `1byte` Modell.
*   Die Größe der aktuellen Seite in Bytes wird berechnet, indem die Länge des HTML-Markups der gesamten Seite ermittelt wird. Anschließend wird der CO2-Ausstoß basierend auf dieser Größe und dem `swd` Modell berechnet.

### Überprüfung des Hostings

*   Der Code überprüft dann, ob das Hosting der aktuellen Domain (`window.location.hostname`) auf grüner Energie basiert, indem er die `hosting.check` Funktion der Bibliothek nutzt. Das Ergebnis dieser Überprüfung wird in einer Variablen gespeichert.

### Ländercodes

Sie können auch jährliche, länderspezifische marginale oder durchschnittliche Netzintensitätsdaten direkt aus CO2.js in Ihre Projekte importieren. In unserem Beispiel wird die durchschnittliche Netzintensität für Österreich (`AUT`) berechnet.

Alle Länder werden durch ihren jeweiligen [Alpha-3 ISO-Ländercode](https://www.iso.org/obp/ui/#search) dargestellt.

### Dynamische Inhalterstellung

*   Eine Hilfsfunktion `addContent` wird definiert, um dynamisch Inhalte zu einem Container-Element auf der Webseite hinzuzufügen. Dieser Container wird durch die ID `co2-calculator-wrapper` identifiziert.
*   Mithilfe dieser Funktion wird für jede der oben berechneten Informationen (CO2-Ausstoß für 1GB, CO2-Ausstoß der aktuellen Seite, Hosting-Informationen, durchschnittliche CO2-Emissionen in Österreich) ein Paragraph (`<p>`) erstellt und dem Container hinzugefügt.

### Integration in eine HTML-Seite

Um diesen Code direkt in eine HTML-Seite zu integrieren, müssen Sie nur 2 Schritte beachten:

1.  **Container-Element hinzufügen**: Fügen Sie in Ihrer HTML-Datei ein Element mit der ID `co2-calculator-wrapper` hinzu, wo die dynamisch generierten Inhalte angezeigt werden sollen.
```html    
<div id="co2-calculator-wrapper"></div>
```        
    
2.  **Script-Tag mit Typ `module` verwenden**: Da der Code das `import`\-Statement verwendet, müssen Sie den Script-Tag in Ihrer HTML-Datei als `type="module"` kennzeichnen, um sicherzustellen, dass der Browser ihn korrekt als ECMAScript-Modul interpretiert.
```html       
 <script type="module">
   // Hier den JavaScript-Code einfügen
 </script>
```
        
    

Indem Sie diese Schritte befolgen, können Sie den JavaScript-Code direkt in jede HTML-Seite integrieren. Beachten Sie, dass für die korrekte Funktion des Codes eine Verbindung zum Internet erforderlich ist, da die Bibliothek von einem externen CDN geladen wird.

Empfehlungen
------------

1.  **Reduzierung digitaler Abhängigkeiten:**
    *   **Evaluierung des Bedarfs:** Bevor Sie neue Abhängigkeiten hinzufügen, bewerten Sie kritisch, ob diese wirklich notwendig sind. Oft können bestehende Tools oder Frameworks bereits die benötigten Funktionen abdecken. Für die Evaluierung des Bedarfs können Tools wie [Bundlephobia](https://bundlephobia.com/) helfen, die Größe von npm-Paketen zu verstehen und zu entscheiden, ob eine neue Abhängigkeit wirklich notwendig ist.
    *   **Statische Webseiten und Generatoren:** Erwägen Sie den Einsatz von statischen HTML Seiten oder Static Site Generators wie [Jekyll](https://jekyllrb.com), [Hugo](https://gohugo.io) oder [Eleventy](https://www.11ty.dev). Statische Seiten laden schneller, sind sicherer und verbrauchen weniger Serverressourcen.
    *   **Minimalistische CMS:** Überlegen Sie den Einsatz von leichtgewichtigen Content-Management-Systemen, die weniger Datenbankinteraktionen erfordern und effizienter sind, wie z.B. [Grav](https://getgrav.org) oder [Kirby](https://getkirby.com).
2.  **Minimalistisches Webdesign und Reduktion clientseitiger Dynamik:**
    *   **Weniger ist mehr:** Nutzen Sie ein Design, das auf das Wesentliche reduziert ist, um die Größe der Webseiten zu verringern und die Ladezeiten zu verbessern.
    *   **Optimierung von Bildern und Medien:** Verwenden Sie moderne, effiziente Bildformate wie WebP und AVIF und setzen Sie Lazy Loading für Bilder und Videos ein. Tools: [Lazysizes](https://afarkas.github.io/lazysizes/), [Squoosh](https://squoosh.app/), [TinyPNG](https://tinypng.com/).
    *   **Vermeidung übermäßiger Skripte:** Verzichten Sie auf schwere JavaScript-Frameworks und Libraries, wenn möglich. Nutzen Sie stattdessen native Web-Technologien und progressive Verbesserung. Inspiration: [Vanilla JS](http://vanilla-js.com/).
3.  **Umweltfreundliches Webhosting:**
    *   **Grünes Hosting wählen:** Suchen Sie nach Hosting-Anbietern, die erneuerbare Energiequellen nutzen und sich für Nachhaltigkeit einsetzen, wie z.B. [GreenGeeks](https://www.greengeeks.com).
    *   **Energieeffizienz bewerten:** Informieren Sie sich über die Energieeffizienz Ihres Datenzentrums an Ihrer Universität oder Ihres Hosters. Best Practie Beispiel ist etwa [Kinsta](https://kinsta.com) für WordPress Seiten, das sich durch hohe Energieeffizienz und den Einsatz erneuerbarer Energien auszeichnet.
    *   **Zertifizierungen und Siegel:** Achten Sie auf relevante Umweltzertifikate und Siegel, die das Engagement des Hosters für Nachhaltigkeit bestätigen. Empfohlen: [The Green Web Foundation](https://www.thegreenwebfoundation.org/).
4.  **Zugänglichkeit und inklusives Design:**
    *   **Barrierefreiheit:** Stellen Sie sicher, dass Ihre Website für Menschen mit unterschiedlichen Fähigkeiten und Bedürfnissen zugänglich ist. Dies schließt die Einhaltung der WCAG (Web Content Accessibility Guidelines) ein. Tools: [axe Accessibility Checker](https://www.deque.com/axe/).
    *   **Responsive Design:** Implementieren Sie responsive Webdesign-Prinzipien, um sicherzustellen, dass Ihre Webseite auf allen Geräten und Bildschirmgrößen gut funktioniert. Tools: [Responsive Web Design Testing Tool](https://responsivedesignchecker.com/). Zusätzlich können Prototyping-Tools wie [InVision](https://www.invisionapp.com/) und [Axure](https://www.axure.com/) helfen, Design- und Usability-Probleme frühzeitig zu erkennen.
    *   **Inklusion von Anfang an:** Berücksichtigen Sie diverse Nutzergruppen in der Planungsphase Ihres Projekts. Diversität in Testgruppen kann helfen, Design- und Usability-Probleme frühzeitig zu identifizieren.
5.  **Performance-Optimierung:**
    *   **Caching-Strategien:** Implementieren Sie effektive Caching-Strategien, um die Belastung der Server zu verringern und schnelle Ladezeiten zu gewährleisten. Tools: [W3 Total Cache](https://wordpress.org/plugins/w3-total-cache/), [Hummingbird](https://wordpress.org/plugins/hummingbird-performance/).
    *   **Content Delivery Networks (CDN):** Nutzen Sie CDNs, um Inhalte geografisch näher an den Nutzern zu speichern und somit die Latenz zu verringern, wie z.B. [Cloudflare](https://www.cloudflare.com).
    *   **Minimierung von Ressourcen:** Minimieren und komprimieren Sie CSS, JavaScript und HTML-Dateien. Vermeiden Sie unnötigen Code und Kommentare in Produktionsdateien. Tools für die Optimierung von Schriftarten: [FontSquirrel](https://www.fontsquirrel.com/), [Everything Fonts](https://everythingfonts.com). Tools für die Performance-Optimierung: [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/), [WebPageTest](https://www.webpagetest.org/), [Critical](https://github.com/addyosmani/critical).
6.  **Nachhaltige Praktiken und Messungen:**
    *   **CO2-Fußabdruck messen:** Verwenden Sie Tools zur Messung des CO2-Fußabdrucks Ihrer Webseite, um Bereiche mit Verbesserungspotenzial zu identifizieren. Ein Beispiel ist der [Website Carbon Calculator](https://www.websitecarbon.com).
    *   **Nachhaltigkeitsberichte:** Erstellen Sie regelmäßige Berichte über die Umweltauswirkungen Ihrer Webprojekte und setzen Sie sich Ziele zur Reduzierung.
    *   **Bildung und Bewusstsein:** Fördern Sie Bewusstsein für Green Web Design innerhalb Ihres Teams und Ihrer Community. Teilen Sie Best Practices und ermutigen Sie zu nachhaltigeren Ansätzen in der Webentwicklung. Ressourcen: [Sustainable Web Design](https://www.sustainablewebdesign.org), [Green IT Webinar Series](https://www.greenit.org/webinars).

Bibliografie
------------

Baillot, A., & Roeder, T. (2023, Dezember 15). _Von der Initiative bis zur Praxis: Die DHd-AG Greening DH und das DHCC Toolkit_. https://doi.org/10.5281/zenodo.10392521

Bender, E. M., Gebru, T., McMillan-Major, A., & Shmitchell, S. (2021). On the Dangers of Stochastic Parrots: Can Language Models Be Too Big? 🦜. _Proceedings of the 2021 ACM Conference on Fairness, Accountability, and Transparency_, 610–623. https://doi.org/10.1145/3442188.3445922

Blauer Engel. (2020). _Ressourcen- und energieeffiziente Softwareprodukte (DE-UZ 215). Vergabekriterien_. https://www.blauer-engel.de/de/produktwelt/ressourcen-und-energieeffiziente-softwareprodukte

Chapman, K. (2022). The conference conundrum. _Chemistry World_. https://www.chemistryworld.com/careers/the-conference-conundrum/4014882.article

_ChatGPT is growing faster than TikTok_. (2023, Februar 1). https://www.cbsnews.com/news/chatgpt-chatbot-tiktok-ai-artificial-intelligence/

_Climate Action Community Work Plan 2022-2023_. (o. J.). Europeana Pro. Abgerufen 1. Februar 2023, von https://pro.europeana.eu/post/climate-action-community-work-plan-2022-2023

_Climate-Friendly Research – AQUACLEW_. (o. J.). Abgerufen 16. Februar 2022, von https://aquaclew.eu/climate-friendly-research/

Community-run open source tools for video, text, and code collaboration. (2020, April 14). _Flavours of Open_. https://flavoursofopen.science/community-run-open-source-tools-for-video-and-text-collaboration/

_Data Centres and Data Transmission Networks – Analysis_. (o. J.). IEA. Abgerufen 31. Mai 2023, von https://www.iea.org/reports/data-centres-and-data-transmission-networks

Directorate-General for Research and Innovation (European Commission). (2023). _Driving a green, digital & innovative European cultural heritage_. Publications Office of the European Union. https://data.europa.eu/doi/10.2777/600577

_ethical\_web\_dev\_web.pdf_. (o. J.). Abgerufen 1. August 2023, von https://edri.org/files/ethical\_web\_dev\_web.pdf

Faber, G. (2021). A framework to estimate emissions from virtual conferences. _International Journal of Environmental Studies_, _78_(4), 608–623. https://doi.org/10.1080/00207233.2020.1864190

Flohr, M. (o. J.). _netzwerk n_. Abgerufen 25. Januar 2022, von https://www.netzwerk-n.org/

Glausiusz, J. (2021). Rethinking travel in a post-pandemic world. _Nature_. https://www.nature.com/articles/d41586-020-03649-8

Gröger, J., & Köhn, M. (2014). _Dokumentation des Fachgesprächs “Nachhaltige Software_. Umweltbundesamt. https://www.umweltbundesamt.de/publikationen/nachhaltige-software

Hauke, P., Latimer, K., & Werner, K. U. (Hrsg.). (2013). _The Green Library - Die grüne Bibliothek: The challenge of environmental sustainability - Ökologische Nachhaltigkeit in der Praxis_. DE GRUYTER SAUR. https://doi.org/10.1515/9783110309720

Höfner, A., Frick, V., Chan, J., Kurz, C., Santarius, T., & Zahrnt, A. (Hrsg.). (2019). _Was Bits und Bäume verbindet: Digitalisierung nachhaltig gestalten ; Bits & Bäume, die Konferenz für Digitalisierung und Nachhaltigkeit, Technische Universität Berlin im November 2018_. oekom verlag.

Holmes, M., & Takeda, J. (2019). _The Prefabricated Website: Who needs a server anyway?_ https://doi.org/10.5281/ZENODO.3449197

_Home_. (2023, Juli 31). The Green Web Foundation. https://www.thegreenwebfoundation.org/

Klöwer, M., Hopkins, D., Allen, M., & Higham, J. (2020). An analysis of ways to decarbonize conference travel after COVID-19. _Nature_. https://www.nature.com/articles/d41586-020-02057-2

Knödlseder, J., Brau-Nogué, S., Coriat, M., Garnier, P., Hughes, A., Martin, P., & Tibaldo, L. (2022). Estimate of the carbon footprint of astronomical research infrastructures. _arXiv:2201.08748 \[astro-ph\]_. http://arxiv.org/abs/2201.08748

Lange, S., & Santarius, T. (2018). _Smarte grüne Welt? Digitalisierung zwischen Überwachung, Konsum und Nachhaltigkeit_. Oekom Verlag.

_Leitfaden zur umweltfreundlichen öffentlichen Beschaffung von Software - Neufassung 2023 - Website kleine kniffe_. (o. J.). Abgerufen 11. August 2023, von https://nachhaltige-beschaffung.com/newsdetails/leitfaden-zur-umweltfreundlichen-%C3%B6ffentlichen-beschaffung-von-software-neufassung-2023.html

_leitfragenkatalog-data.pdf_. (o. J.). Abgerufen 21. Dezember 2023, von https://www.dfg.de/resource/blob/289478/c77e5c2d7993892e9e19b8d1375e1af7/leitfragenkatalog-data.pdf

Mariette, J., Blanchard, O., Berné, O., & Ben-Ari, T. (2021). _An open-source tool to assess the carbon footprint of research_ \[Preprint\]. Scientific Communication and Education. https://doi.org/10.1101/2021.01.14.426384

Meunier, C. (2016). _Sustainable Software Design_. Umweltbundesamt. https://www.umweltbundesamt.de/publikationen/sustainable-software-design

Netzwerk Grüne Bibliothek. (o. J.). _Bibliografie Grüne Bibliothek_. Netzwerk Grüne Bibliothek. https://www.netzwerk-gruene-bibliothek.de/bibliografie/

Örtl, E. (2018). _Entwicklung und Anwendung von Bewertungsgrundlagen für ressourceneffiziente Software unter Berücksichtigung bestehender Methodik_. Umweltbundesamt. https://www.umweltbundesamt.de/publikationen/entwicklung-anwendung-von-bewertungsgrundlagen-fuer

Patterson, D., Gonzalez, J., Le, Q., Liang, C., Munguia, L.-M., Rothchild, D., So, D., Texier, M., & Dean, J. (o. J.). _Carbon Emissions and Large Neural Network Training_.

Pendergrass, K. L., Sampson, W., Walsh, T., & Alagna, L. (2019). Toward Environmentally Sustainable Digital Preservation. _The American Archivist_, _82_(1), 165–206. https://doi.org/10.17723/0360-9081-82.1.165

published, T. P. (2022, Februar 2). _The mission to reduce the carbon footprint of astronomy_. Space.Com. https://www.space.com/reducing-carbon-footprint-of-astronomy

Ramesohl, S. (2020, Oktober 22). _Digitalisierung und Klimawandel_. Die Junge Akademie - KlimaLectures #3, online. https://www.youtube.com/watch?v=I2U8YqJYy3E

Report: Digital Reset. (o. J.). _D4S - digitalization for sustainability_. Abgerufen 20. Oktober 2022, von https://digitalization-for-sustainability.com/digital-reset/

Roussihe, G. (2021, Januar 23). _Explications sur l’empreinte carbone du streaming et du transfert de données_. Blog. http://gauthierroussilhe.com/post/explication-streaming.html

Schomaker, G., Janacek, S., & Schlitt, D. (2015). The Energy Demand of Data Centers. In L. M. Hilty & B. Aebischer (Hrsg.), _ICT Innovations for Sustainability_ (Bd. 310, S. 113–124). Springer International Publishing. https://doi.org/10.1007/978-3-319-09228-7\_6

Stallmann, M. (2020). _Guide on Green Public Procurement of Software_. Umweltbundesamt. https://www.umweltbundesamt.de/publikationen/guide-on-green-public-procurement-of-software

Stroud, J. T., & Feeley, K. J. (2015). Responsible academia: optimizing conference locations to minimize greenhouse gas emissions. _Ecography_, _38_(4), 402–404. https://doi.org/10.1111/ecog.01366

Tao, Y., Steckel, D., Klemeš, J. J., & You, F. (2021). Trend towards virtual and hybrid conferences may be an effective climate change mitigation strategy. _Nature Communications_, _12_(1), 7324. https://doi.org/10.1038/s41467-021-27251-2

Thaller, A., Schreuer, A., & Posch, A. (2021). Flying High in Academia—Willingness of University Staff to Perform Low-Carbon Behavior Change in Business Travel. _Frontiers in Sustainability_, _2_. https://doi.org/10.3389/frsus.2021.790807

The Green Web Foundation. (o. J.). _The Green Web Foundation_. https://www.thegreenwebfoundation.org/

Urai, A. E., & Kelly, C. (2023). Rethinking academia in a time of climate crisis. _eLife_, _12_, e84991. https://doi.org/10.7554/eLife.84991

van Ewijk, S., & Hoekman, P. (2021). Emission reduction potentials for academic conference travel. _Journal of Industrial Ecology_, _25_(3), 778–788. https://doi.org/10.1111/jiec.13079

Wissen, V. B. H. K., & Technik. (2023, Februar 23). _Verschlimmert ChatGPT die Klimakrise? Experten warnen_. Utopia.de. https://utopia.de/chat-gpt-und-die-klimakrise-experten-warnen-474199/
