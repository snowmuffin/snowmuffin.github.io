---
layout: post
title: TourSherpa
categories: [projects]
date: 2024-08-12 00:50 +0900
---
[https://github.com/Toursherpa/TourSherpa.git](https://github.com/Toursherpa/TourSherpa.git)
# 여행 보조 프로그램

## 프로젝트 개요

이 프로젝트는 여행지 주변의 행사 정보를 바탕으로 비행기편, 숙소, 식당 등을 추천해주는 여행 보조 프로그램입니다. 사용자는 특정 지역의 행사 정보를 통해 더 나은 여행 계획을 세울 수 있으며, 이를 바탕으로 항공편과 숙소, 그리고 식당까지 추천받을 수 있습니다.

이 프로그램은 두 가지 주요 컴포넌트로 구성됩니다:

1. **Django**: 사용자 인터페이스를 제공하며, AWS RDS에서 데이터를 가져와 사용자에게 정보를 제공합니다.
2. **Airflow**: 다양한 API에서 데이터를 수집하여 AWS S3에 저장한 뒤, AWS Redshift로 적재하여 Django가 이를 사용하도록 합니다.

프로젝트는 Docker 컨테이너를 통해 Airflow와 Django가 각각의 인스턴스로 구성되며, AWS에서 호스팅됩니다.

## 시스템 아키텍처

![시스템 아키텍처 다이어그램]

위의 다이어그램은 API에서 수집된 데이터가 Airflow를 통해 AWS S3 및 RDS에 저장되고, Django를 통해 사용자에게 제공되는 구조를 보여줍니다.

## ERD (Entity-Relationship Diagram)

프로젝트의 데이터베이스 구조를 시각화한 ERD는 아래와 같습니다:

![ERD](assets/img/ERD.png)

## 주요 기능

### 1. 행사 정보 수집 및 추천
이 프로그램은 Predicthq API를 사용하여 전 세계의 행사 정보를 수집하고, 이를 기반으로 사용자에게 관련 행사와 추천 정보를 제공합니다. 이 데이터는 Airflow에서 자동으로 수집 및 처리되어 AWS Redshift에 저장되며, Django가 이를 활용하여 사용자에게 실시간으로 정보를 제공합니다.

### 2. 항공편 및 숙소 정보 제공
사용자가 선택한 행사에 근접한 항공편과 숙소 정보를 제공하기 위해 Amadeus API와 Google Place API를 활용합니다. 항공편 정보는 `FlightTo` 및 `FlightFrom` 모델로 관리되며, 숙소 정보는 `HotelList` 모델로 관리됩니다. 사용자는 특정 행사와 관련된 항공편 및 숙소 정보를 쉽게 조회할 수 있습니다.

### 3. 데이터 관리
모든 데이터는 AWS Redshift를 통해 관리되며, Django의 ORM을 통해 필요에 따라 데이터를 조회하고 가공합니다. 이를 통해 대용량 데이터도 효율적으로 관리할 수 있습니다.

## 설치 및 설정 가이드

### 1. 환경 변수 설정
프로젝트 실행에 필요한 환경 변수는 `variables.json` 파일에 정의되어 있으며, 아래와 같은 형식으로 설정됩니다:

```json
{
    "GOOGLE_API_KEY": "YOUR_GOOGLE_API_KEY",
    "predicthq_ACCESS_TOKEN": "YOUR_PREDICTHQ_ACCESS_TOKEN",
    "redshift_schema_places": "redshift_schema_places",
    "redshift_table_nearest": "nearest_airports",
    "redshift_table_places": "events_places",
    "s3_bucket_name": "s3_bucket_name"
}

2. Docker 컨테이너 실행

프로젝트는 Docker를 통해 실행됩니다. Docker Compose 파일을 사용하여 Airflow와 Django를 구성하며, AWS에 배포된 인프라와 연결합니다.
3. AWS 설정

AWS S3, RDS, 그리고 Redshift를 활용하여 데이터를 관리합니다. 아래는 주요 설정 정보입니다:

    AWS S3: s3_bucket_name 버킷을 사용하여 CSV 데이터를 저장합니다.
    AWS Redshift: 대용량 데이터는 Redshift 클러스터에 저장되어 분석됩니다.

사용법
1. 대시보드

dashboard 페이지는 사용자가 접근할 수 있는 대시보드 화면을 제공합니다. 여기서는 다양한 차트와 통계를 통해 현재 인기 있는 행사, 항공권 가격, 행사 분포 등을 시각화하여 보여줍니다.

    국가별 인기 행사: 사용자가 선택한 국가에서 가장 인기 있는 행사를 차트로 보여줍니다.
    카테고리별 이벤트 수: 이벤트의 카테고리에 따라 이벤트 수를 비교하여 보여줍니다.
    항공권이 가장 싼 국가: 평균 항공권 가격이 가장 저렴한 국가를 시각화합니다.

2. 국가별 행사 리스트

country.html 파일을 통해 특정 국가의 주요 행사를 목록으로 제공하며, 차트로 지역별 행사 수와 카테고리별 행사 수를 시각화합니다. 사용자는 국가별 행사 목록을 클릭하여 세부 정보를 확인할 수 있습니다.
3. 행사 상세 정보

event_detail 페이지는 사용자가 특정 행사를 클릭하면 해당 행사에 대한 상세 정보를 제공하는 화면입니다. 여기에는 행사 설명, 예상 참가자 수, 관련 항공편 및 숙소 정보가 포함됩니다.

    항공편 정보: 사용자는 행사 근처의 공항과 연관된 항공편 정보를 확인할 수 있으며, Skyscanner 링크를 통해 실제 항공편 예약이 가능합니다.
    숙소 정보: 행사 근처의 숙소 정보를 제공하며, 숙소 평점 및 리뷰를 확인할 수 있습니다.

4. 숙소 상세 정보

hotel_detail 페이지는 특정 숙소의 상세 정보를 제공하며, 관련 행사 목록을 함께 보여줍니다. 사용자는 숙소의 평점, 리뷰 수, 체크인/체크아웃 시간 등 정보를 확인할 수 있으며, 달력을 통해 예약 가능 여부를 확인할 수 있습니다.