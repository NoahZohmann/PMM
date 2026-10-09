# Antwort

Gute Frage – hier eine thematische Einordnung, was in RStudio wirklich Zeit spart und wo der Taschenrechner weiterhin reicht.

## Kennzahlen (Mittelwert, Standardabweichung, Median, …)

**By hand:** Für jede Kennzahl eine eigene Formel, Summen über alle Werte – bei n>10 fehleranfällig und langsam. Median und Quartile erfordern Sortieren und Abzählen.

**R-Vorteil:** Mit einem Vektor alle Kennzahlen auf einen Schlag:

```r
x <- c(12, 15, 11, 18, 22, 15, 19, 14)
mean(x); median(x); sd(x); var(x); quantile(x)
summary(x)          # Min, Q1, Median, Mean, Q3, Max in einer Zeile
```

Besonders stark: ganze Datensätze statt einzelner Listen, gruppenweise Kennzahlen (`aggregate`, `dplyr::summarise`), fehlende Werte (`na.rm=TRUE`).

## Boxplot

**By hand:** Fünfzahlen-Zusammenfassung berechnen, Ausreißerregel (1,5·IQR) prüfen, dann maßstabsgetreu zeichnen – sehr aufwändig.

**R-Vorteil:** Ein Befehl, korrekt skaliert, inkl. Ausreißer:

```r
boxplot(x, horizontal = TRUE)
boxplot(Wert ~ Gruppe, data = df)   # Vergleich mehrerer Gruppen
```

## Diagramme (Histogramm, Kreisdiagramm, …)

**By hand:** Klassen bilden, Häufigkeiten auszählen, Winkel für Kreisdiagramm (Prozent·3,6°) – reine Fleißarbeit ohne Erkenntnisgewinn.

**R-Vorteil:** Klassierung automatisch, beliebig anpassbar:

```r
hist(x, breaks = "Sturges")
pie(table(kategorie))
barplot(table(kategorie))
```

Plus: Skalierung, Achsen, Titel, Export als Bild – alles gratis.

## Binomialverteilung

**By hand:** P(X=k) = C(n,k)·p^k·(1-p)^(n-k) – der Binomialkoeffizient ist mühsam, Summen (P(X≤k)) erst recht.

**R-Vorteil:** Vier Funktionen, kein Rechnen nötig:

```r
dbinom(3, size = 10, prob = 0.5)        # P(X = 3)
pbinom(3, size = 10, prob = 0.5)        # P(X ≤ 3)
qbinom(0.95, size = 10, prob = 0.5)     # Quantil
rbinom(10, size = 10, prob = 0.5)       # Zufallswerte
```

## Normalverteilung (Dichte- und Verteilungsfunktion)

**By hand:** Kein geschlossenes Integral – man braucht z-Tabelle (oder Plot). Jede Aufgabe = erst z = (x−μ)/σ, dann Tabelle ablesen.

**R-Vorteil:** Direkt mit μ und σ, ohne Standardisierung und ohne Tabelle:

```r
dnorm(1.5, mean = 0, sd = 1)            # Dichtewert f(x)
pnorm(1.5, mean = 100, sd = 15)         # P(X ≤ 1.5)  bzw. Fläche links
qnorm(0.975, mean = 0, sd = 1)          # kritischer Wert (z ≈ 1.96)
xp <- seq(-4, 4, 0.01); plot(xp, dnorm(xp), type = "l")   # Dichtekurve
```

Intervall-/Flächenaufgaben: `pnorm(b)−pnorm(a)` statt Tabellensuche und Subtraktion.

## t-Verteilung

**By hand:** Für jeden Freiheitsgrad (df) eine andere Tabelle – sehr lästig, oft nur ausgewählte Spalten.

**R-Vorteil:** Beliebiges df, kein Tabellenwechsel:

```r
dt(2, df = 9); pt(2, df = 9); qt(0.975, df = 9)
```

## Fazit – wo R gewinnt vs. wo Taschenrechner reicht

| Thema | R läuft deutlich schneller/besser | Taschenrechner reicht |
|---|---|---|
| Kennzahlen | große/mehrere Datensätze, gruppiert | 5–8 Zahlen, einfacher Mittelwert |
| Boxplot | immer (Zeichnen+Skalierung) | praktisch nur grob skizziert |
| Histogramm/Kreisdiagramm | immer | zeichnerisch grob |
| Binomialverteilung | Summen, n groß | einzelne P(X=k) mit kleiner Zahl |
| Normalverteilung | immer (keine z-Tabelle nötig) | Flächen grob schätzen |
| t-Verteilung | immer (beliebiges df) | eine tabellierte Spalte ablesen |

Kurz gesagt: Der Taschenrechner gewinnt nur bei einzelnen, kleinen Rechnungen. Sobald es **viele Werte, Verteilungsfunktionen, Quantile, Grafiken oder Vergleiche** gibt, ist RStudio klar überlegen – vor allem, weil Rechen- und Tabellenfehler entfallen.

Wenn du möchtest, lege ich dir ein lauffähiges R-Skript mit allen Beispielen oben an (z.B. `Statistik_Demo.R`) – sag einfach Bescheid.
