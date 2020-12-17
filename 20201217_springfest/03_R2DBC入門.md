# R2DBC入門

VMWare
@ttddyy(Twitter, Github)

## What is R2DBC
Reactive Relational Database Connectivity

 - Standard Specification
 - Reactive non-blocking API

## Gackground
Emergence of Reactive Programming

## What is R2DBC
- リアクティブプログラミング標準API
- 実装依存しない
Non-blocking
Reactive Stream(Publisher, Subscriber)


## R2DBC
R2DBCは仕様
- ドキュメント
- SPI (仕様を実装に落とし込んだもの)
  インターフェースと簡単なユーティリティクラス

## JDBC and R2DBC SPI comparision

JDBC          : R2DBC
DataSource    : 


## Simple Select (with SPI)

Publisher<Integer> result = connectionFactory.create()
				.flatMapMany(connection ->
				    connection
					.createStatement("SELECT id, name FROM test")
					.execute()
					.flatMap(result ->
					    result.map((row, metadata) ->
						row.get("id", Integer.class))));


## Accessing Database

Application
-> 

Client        - Highlevel API
Tool (SPI)    - Connection Pool, Proxy, etc
Driver (SPI)  - Each Database

->
DB

## R2DBC Driver Implementation

 - PostgreSQL
 - MySQL
 - MariaDB
 - H2
 - MicrosoftSQL Server
など

 - Oracle(Work in progress)
 - DB2(Work in progress)


## R2DBC Tools

 - R2DBC Pool
    - connection pooling
 - R2DBC Proxy
    - Callback API
      - Observability : 分散トレージングでスパンを作るときとかに使ったり
      - Metrics
      - Logging


## R2DBC Client
- Spring - R2DBC Migrate
- Spring Data   - Testcontainers

(Work In Progress)
 - jOOQ
 - Querydsl
 - MyBatis
 - JetBrain Exposed
 - JDBI
 - Liquibase
 - Micronaut
 - Flyway


## Spring for R2DBC

spring-r2dbc module
  - Spring Framework 5.3 (Spring Boot 2.4)から

DatabaseClient(Spring)

return client.sql("SELECT id, name FROM test WHERE name = :name")
	.bind("name", name)
	.map((row, metadata) -> row.get(0, Integer.class))
	.all();


## R2DBC RoadMap

Reactive Foundationに移行した
ベンダーロックインではなく中立な立場

バージョン
- 0.8 (Current)
  - Stable
- 0.9
  - Small additions
(大体の機能はそろった)

- 1.0 Release
  - Relabel 0.9 once stability is proven

## Resources
Project
  - Homepage: https://r2dbc.io
  - Github:  https://github.com/r2dbc



