---
title: azcopy doc | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy doc.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: d1f57ae40b47c1d910ba20ae8a8f19cdc6908d6b
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196838"
---
# <a name="azcopy-doc"></a>azcopy doc

Genera documentación para la herramienta en formato de Markdown.

## <a name="synopsis"></a>Sinopsis

Genera documentación para la herramienta en formato Markdown y la almacena en la ubicación designada.

De forma predeterminada, los archivos se almacenan en una carpeta denominada "doc" en el directorio actual.

```azcopy
azcopy doc [flags]
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|-h, --help|Muestra el contenido de la ayuda para el comando doc.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
