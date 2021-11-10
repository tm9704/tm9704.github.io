---
title: "Java(57) - 자바 입출력 스트림"
excerpt: "Java IO Stream"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-11-10 16:59:00"
last_modified_at: "2021-11-10 17:30:00"
---

## 1. 입출력 스트림이란?

- 네트워크에서 자료의 흐름이 물과 같다는 의미에서 유래
- 다양한 입출력 장치에 독립적으로 일관성 있는 입출력 방식을 제공<br/>
  ![IOStream](/images/IOStream.png)
- 입출력이 구현되는 곳에서는 모두 I/O 스트림을 사용<br/>
  (키보드, 파일디스크, 메모리 등..)

## 2. 입출력 스트림 구분

- I/O 대상 기준 : 입력 스트림, 출력 스트림<br/>
  (입출력 동시는 없음)
- 자료의 종류 : 바이트 스트림, 문자 스트림
- 스트림의 기능 : 기반 스트림(base), 보조 스트림(기능 보조, 읽거나 쓰는 기능은 없음)

## 3. 입출력 스트림과 출력 스트림

- 입력 스트림 : 대상으로부터 자료를 읽어 들이는 스트림
- 출력 스트림 : 대상으로 자료를 출력하는 스트림

![java_iostream](/images/java_iostream.png)<br/>

- 스트림의 예<br/>
  1. 입력스트림 : FileInputStream, FileReader, BufferedInputStream, BufferedReader..
  2. 출력스트림 : FileOutputStream, FileWriter, BufferedOutputStream, BufferedWriter..

## 4. 바이트 단위 스트림과 문자 단위 스트림

- 바이트 단위 스트림<br/>
  바이트 단위로 자료를 읽고 씀(동영상, 음악파일 등)
- 문자 단위 스트림<br/>
  문자는 2바이트씩 처리해야 함

![byte_char_stream](/images/byte_char_stream.png)<br/>

- 종류<br/>
  1. 바이트 스트림 : FileInputStream, FileOutputStream, BufferedInputStream, BufferedOutputStream..
  2. 문자 스트림 : FileReader, FileWriter, BufferedReader, BufferedWriter..

## 5. 기반 스트림과 보조 스트림

- 기반 스트림 : 대상에 직접 자료를 읽고 쓰는 기능(Reader, Writer)의 스트림
- 보조 스트림 : 직접 읽고 쓰는 기능은 없고, 추가적인 기능을 제공해 주는 스트림,<br/>
  기반 스트림이나 또 다른 보조 스트림을 생성자의 매개변수로 포함함<br/>
  ![decorator](/images/decorator.png)
- 스트림의 예<br/>
  1. 기반 스트림 : FileInputStream, FileOutputStream, FileReader, FileWriter..
  2. 보조 스트림 : InputStreamReader, OutputStreamWriter, BufferedInputStream, BufferedOutputStream..
