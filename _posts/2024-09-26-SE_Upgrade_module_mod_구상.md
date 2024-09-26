---
layout: post
title: SE_Upgrade_module_mod
date: 2024-09-26 16:43 +0900
categories: [projects, Space_Engineers_SEK_Server]
tags: [server, steam]
projects: [Space_Engineers_SEK_Server]
---
이 게임의 블럭이나 아이템은 이미 정의된 XML파일에서 설정된 값을 게임이 시작되기 전에 로드한다. 해당 값들은 런타임 도중에 동적으로 생성되거나 변경하는것은 구조적으로 불가능하다. 

같은 종류의 객체들을 미리 세분화하여 정의하는 방법은 명확한 한계를 가진다. 이 모드의 목적은 유저가 특정한 아이템을 강화하면 각 아이템마다 랜덤한 강화가 적용되게 하는것이다. 모든 강화의 경우의 수많큼 xml에서 subtypeid를 정의하는 것은 확장성, 유지보수를 어렵게 하고 프로그램 성능에 악영향을 줄것으로 보인다. 

Space engineers는 각 블럭이나 아이템에 대해 CustomData필드를 지원한기에 이를 서버에서 생성하고 저장하는 방식을 채택하는 방향으로 시도하면 가능성이 있을것으로 보인다.
https://github.com/snowmuffin/SE_Upgrade_module_mod