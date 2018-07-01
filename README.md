# Salud-datos2

toCamelCase[string_String] := 
  StringJoin[
   StringReplace[ToLowerCase[StringSplit[string, "_"]], 
    WordBoundary ~~ x_ :> ToUpperCase[x]]];

rawSourceData = 
 Import[FileNameJoin[{NotebookDirectory[], 
    "DAT Morbilidad_Adolescente.csv"}], "csv"]

rawProperties = First[rawSourceData]

StringReplace[
 toCamelCase /@ rawProperties, {"Anio" -> "Año", 
  "Nromes" -> "NúmeroDelMes", "Casos1014" -> "CasosDe10a14", 
  "Casos1519" -> "CasosDe15a19", "Casos1217" -> "CasosDe12a17"}]

rawData = Rest[rawSourceData]

rawDataAssociation = AssociationThread[properties -> #] & /@ rawData

If[#[[4]] == "MOQUEGUA", #[[8]], ""] & /@ rawDataAssociation // Tally

If[#[["Sexo"]] == "F", #[[8]], ""] & /@ rawDataAssociation // Tally

If[#[["Sexo"]] == "M", #[[8]], ""] & /@ rawDataAssociation // Tally

If[#[["NúmeroDelMes"]] == 1, #[[8]], ""] & /@ 
  rawDataAssociation // Tally
