---
title: 온라인 호스팅 지침
permalink: index.html
layout: home
ms.openlocfilehash: 928f59a9cdc6a3f70d5ad651fb1b5a45b405cee8
ms.sourcegitcommit: 1117342052bce0bbd5a703bd1f763862b9129807
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2022
ms.locfileid: "140682429"
---
# <a name="azure-data-fundamentals-exercises"></a>Azure Data Fundamentals 연습

아래 링크를 사용하여 Microsoft 과정 [DP-900 *Microsoft Azure Data Fundamentals*](https://docs.microsoft.com/learn/certifications/courses/dp-900t00)에 대한 실습 랩 연습을 완료합니다.

이 연습을 완료하려면 Microsoft Azure 구독이 필요합니다. 강사가 Microsoft Azure 구독을 제공하지 않은 경우 [https://azure.microsoft.com](https://azure.microsoft.com)에서 무료 평가판에 등록할 수 있습니다.

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| 모듈 | 랩 |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
