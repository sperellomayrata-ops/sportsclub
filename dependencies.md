# Auditoria de Vulnerabilitats i Monitoratge

## 1. GitHub Dependabot
He activat **Dependabot** al repositori. Aquesta eina realitza un escaneig passiu i continu del nostre fitxer `requirements.txt`, notificant vulnerabilitats conegudes mitjançant la base de dades de seguretat de GitHub i generant Pull Requests automàtics per corregir-les.

## 2. Auditoria Local amb Safety
Ja que va ser l'eina que vaig emprar per fer la presentació de la pràctica anterior, he optat per **Safety**, una eina líder específica per a l'ecosistema Python.

### Resultats de l'Escaneig:
En executar `safety check -r requirements.txt`, s'han detectat **9 vulnerabilitats**.
* **Descobriment Crític:** S'ha identificat que **Django 6.0** conté múltiples vulnerabilitats de seguretat conegudes.
* **Remei:** L'informe suggereix actualitzar a **Django 6.0.2**. Aquest descobriment demostra la importància de realitzar auditories abans de passar a producció, ja que el codi pot ser correcte però els seus fonaments (les llibreries) no ho són.

## 3. Alternatives per a Núvol Privat (Self-hosted)
En cas de no disposar dels serveis de GitHub, aquestes són les tres alternatives recomanades:

| Eina | Ús Recomanat | Avantatges |
| :--- | :--- | :--- |
| **Safety** | Auditoria ràpida de Python | Integració nativa amb pip i molt lleugera. |
| **Pip-audit** | Entorns oficials PyPA | Utilitza la base de dades OSV (Open Source Vulnerability). |
| **Snyk (Binari)** | Auditoria integral 360º | Analitza no només llibreries, sinó també codi i contenidors. |

## 4. Conclusió Tècnica
La implementació d'un **Lock File** amb hashes, juntament amb l'ús d'eines d'escaneig com **Safety**, transforma un desplegament fràgil en un procés robust i segur. La detecció de vulnerabilitats en Django en aquesta pràctica és una prova real de per què aquestes eines són indispensables en qualsevol pipeline de CI/CD.