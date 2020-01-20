---
# 포스트 제목을 기재합니다.
title:  "TypeORM with NestJS" 
# 저자를 입력합니다. 
author: "mansour"
# 카테고리를 입력합니다.
categories: ["TypeORM"]
last_modified_at: 2020-01-20T08:15:00-18:00
---

## ORM이란

ORM (Object-relational mapping)은 객체지향 프로그래밍(Object-Oriented-Programming)과   

관계형 데이터베이스(Relational-Database)사이의 호환되지 않는 데이터를 변환하는 프로그래밍 기법이다.   
      
            
## TypeORM 
   
![https://github.com/typeorm/typeorm/raw/master/resources/logo_big.png](https://github.com/typeorm/typeorm/raw/master/resources/logo_big.png)

TypeScript와 JavaScript(ES5, ES6, ES7) 용 ORM인 TypeORM을 사용한다.   

TypeORM은 MySQL, PostgreSQL, MariaDB, SQLite, MS SQL Server, Oracle, WebSQL 데이터베이스를 지원한다.


NestJS 프로젝트에서 적용한 예제이다. (MySQL v5.7.28 사용)

[TypeORM](https://typeorm.io/#/)   

[NestJS](https://docs.nestjs.com/)

## Install

Nest 에서는 **@nestjs/typeorm**  package 를 제공한다.   

준비한 프로젝트의 터미널에서 아래 코드를 입력한다.
    
        npm install --save @nestjs/typeorm typeorm mysql
   

## Setting

패키지의 설치가 완성되면 우리 프로젝트의 root의 `AppModule` 에서 `TypeOrmModule` 을 import 할 수 있는데,

`forRoot()` 함수에 파라미터로 만들어서 TypeORM의 `createConnection()` 과 같은 설정을 해줄 수 있다.

여기서 DB 접근 정보를 파라미터로 주지 않으면 root경로의 `ormconfig.json`의 파일에서 설정 값을 자동으로 찾아 사용한다.


        //app.module.ts
        
        import { Module } from '@nestjs/common';
        import { TypeOrmModule } from '@nestjs/typeorm';
        import { Photo } from './photo/photo.entity';
        
        @Module({
          imports: [
            TypeOrmModule.forRoot({
              type: 'mysql',
              host: 'localhost',
              port: 3306,
              username: 'root',
              password: 'root',
              database: 'test',
              entities: [],
              synchronize: true,
            }),
          ],
        })
        export class AppModule {}
        

## Entity

저장할 데이터의 entity 를 만들어준다. 

        //photo.entity.ts
            
        import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';
        
        @Entity()
        export class Photo {
          @PrimaryGeneratedColumn()
          id: number;
        
          @Column({ length: 500 })
          name: string;
        
          @Column('text')
          description: string;
        
          @Column()
          filename: string;
        
          @Column('int')
          views: number;
        
          @Column()
          isPublished: boolean;
        }

   
> **TypeORM** 에서는 repository design pattern 을 지원하며, 각 entity는 자신의 repository 를 가진다.
>
> 그리고 이 respositories 들은 database 연결로 인해 얻을 수 있다.

   

만들어준 entity는 사용한다는 것을 이전에 작성햇던 `AppModule` 의 `forRoot()` 에 적어서 database에 알려줘야한다.   

그러나 만들 때마다 적는 것이 어렵기 때문에 
        
        //entities: [Photo] 
        entities: [__dirname + '/**/*.entity{.ts,.js}'],
    
    
 와 같이 파일명으로 모두 처리해준다.


## 사용

위의 `Photo` entity 를 사용하고자 임의의 `PhotoService`를 만들어 보았다. 
 

        import { Injectable } from '@nestjs/common';
        import { InjectRepository } from '@nestjs/typeorm';
        import { Repository } from 'typeorm';
        import { Photo } from './photo.entity';
        
        @Injectable()
        export class PhotoService {
          constructor(
            @InjectRepository(Photo)
            private readonly photoRepository: Repository<Photo>,
          ) {}
        
          findAll(): Promise<Photo[]> {
            return this.photoRepository.find();
          }
        }
        


> 각 Module 로 관리 할 때,   
>
> Photo Module 의 imports 파라미터에 [TypeOrmModule.forFeature([Photo])], 를 추가 해주어야 한다. 


database 연결이 성공했다면, `constructor` 에 선언한 `photoRepository`로 쉽게 `photo` entity 에 접근할 수 있다.    

#### Get Data from database

        // repository 에 저장된걸 모두 들고온다.
        this.photoRepository.find();
        
        // repository에서 지정된 id로 가져온다. 
        this.photoRepository.findById(second_photo_id);

#### Push data into database
        
        // repository 에 새 데이터를 넣어준다.
        this.photoRepository.save(profilePhoto);


이외에도 `TypeORM`에서 제공하는 `querybuilder` 등으로 여러가지 복잡한 쿼리들을 비교적 쉽게 작성 할 수 있다.



        
