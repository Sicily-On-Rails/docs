# Dati strutturati
### Cosa sono i dati strutturati
La ricerca sul web(per esempio su Google) si impegna a comprendere i contenuti della pagina. La ricerca può essere aiutata fornendo informazioni esplicite sul significato della pagina includendo i dati strutturati nella pagina. 

I dati strutturati sono un formato standardizzato per fornire informazioni su una pagina e classificarne il contenuto; ad esempio, nella pagina di una ricetta sono gli ingredienti, il tempo di cottura, le calorie e cosi via.

Google utilizza i dati strutturati che trova sul web per comprendere il contenuto della pagina.

### Linee guida generali  sui dati strutturati

#### Linee guida tecniche
- Puoi testare la conformità con le linee guida tecniche usando il [test dei risultati multimediali](https://search.google.com/test/rich-results?hl=it) e lo [strumento Controllo URL](https://support.google.com/webmasters/answer/9012289?hl=it) 

#### Formato
Affinchè le pagine del sito siano idonee per i risultati multimediali, devi eseguire il markup con uno dei tre formati supportati:

- [JSON-LD(consigliato)](https://json-ld.org/)
- Microdati
- RDFa

La maggior parte dei dati strutturati di ricerca utilizza il vocabolario [schema.org](https://schema.org/) che per essere utilizzato bisogna utilizzare una delle codifiche sopra elencate.


#### Accesso
Non bloccare le pagine dei dati strutturati a Googlebot con file robot.txt, il tag noindex o altri metodi di controllo dell'accesso.

**Esempio di dati strutturati per attività locali**

```json
    <script type="application/ld+json">
      {
        "@context": "https://schema.org",
        "@type": "LocalBusiness",
        "image": [
            "https://www.amvidealab.com/assets/bg/amv-idealab-business.webp",
            "https://www.amvidealab.com/assets/bg/amv-idealab-analytics.webp"
        ], 
        "name": "AMV Idealab s.r.l",
        "address": {
          "@type": "PostalAddress",
          "streetAddress": "Zona Industriale SN",
          "addressLocality": "Piraino",
          "addressRegion": "ME",
          "postalCode": "98060",
          "addressCountry": "IT"
        },

        "geo": {
          "@type": "GeoCoordinates",
          "latitude": 38.164919,
          "longitude": 14.850504 
        },
  
        "url": "https://www.amvidealab.com/",
        "telephone": "+390941329793",
        
        "openingHoursSpecification": [
            {
              "@type": "OpeningHoursSpecification",
              "dayOfWeek": [
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday",
                "Saturday"
              ],
              "opens": "9:00",
              "close": "13:00"
            },
            
            {
              "@type": "OpeningHoursSpecification",
              "dayOfWeek": [
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday",
                "Saturday"
              ],
              "opens": "14:30",
              "close": "18:30"
            }
        ]

      }
    </script>

```