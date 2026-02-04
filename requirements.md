# Informe de Seguretat: Gestió de Dependències i Cicle de Vida

## 1. Anàlisi del Fitxer Original
El fitxer `requirements.txt` inicial presentava deficiències crítiques segons els estàndards de seguretat actuals:
* **Manca d'Aïllament:** En no incloure dependències transitives ni versions fixes, l'entorn era inconsistent i difícil de reproduir.
* **Vulnerabilitat d'Integritat:** L'absència de hashes permetia riscos d'atacs a la cadena de subministrament (Supply Chain Attacks), ja que no es podia verificar si el paquet descarregat havia estat manipulat en el repositori públic.

## 2. Solucions Implementades
Per mitigar aquests riscos, he aplicat dues tècniques fonamentals:
* **Version Pinning (`==`):** He fixat les versions exactes per garantir que el programari no canviï de manera imprevista entre entorns. Encara que amb la actualització que ens va proporcionar el professor, aquest punt ja estava completat.
* **Hashing (`--hash`):** He generat signatures SHA-256 per a cada paquet. Això garanteix que el contingut del paquet sigui exactament l'esperat i no hagi estat alterat.

## 3. Metodologia de Millora (pip-tools)
He utilitzat l'eina professional **pip-tools** per professionalitzar la gestió:
1. He creat un fitxer d'entrada (`requirements.in`) amb les dependències directes.
2. He generat el **Lock File** segur mitjançant la comanda:
   `pip-compile --generate-hashes --output-file=requirements.txt requirements.in`

## 4. Procediment en Entorns de Producció
En un entorn real, el flux de treball ha de ser:
1. Actualitzar les dependències en un entorn virtual aïllat.
2. Executar auditories de seguretat abans de qualsevol desplegament.
3. Validar els hashes al pipeline de CI/CD; si un hash no coincideix, el desplegament s'ha d'avortar automàticament.