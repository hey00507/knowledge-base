---
tags:
  - work/dev
related: null
---
# 표준 사전 테이블 DDL (계속)

### 표준코드사전 임시 데이터 (계속)

```sql
-- 표준코드사전 임시 데이터 (10개)
INSERT INTO "usc_pt".tb_dg_std_code_list (
    sn_no, mng_dept_nm, code_korn_nm, code_eng_abbr_nm, code_eng_nm,
    code_expln_cn, data_typ_nm, data_len_cnt, code_vl_cn, 
    cd_vl_mn_cn, code_vl_expl_cn, reg_dt, rgtr_nm
) VALUES
(1, '시스템관리팀', '사용여부코드', 'USE_YN', 'UseYnCode', '시스템 내 데이터의 사용 상태를 나타내는 코드', 'CHAR', '1', 'Y,N', '사용,미사용', 'Y=사용가능한 상태, N=사용불가능한 상태', NOW(), 'system'),
(2, '시스템관리팀', '삭제여부코드', 'DEL_YN', 'DeleteYnCode', '데이터의 삭제 상태를 나타내는 코드', 'CHAR', '1', 'Y,N', '삭제,활성', 'Y=삭제된 상태, N=활성 상태', NOW(), 'system'),
(3, '사용자관리팀', '사용자등급코드', 'USER_GRD', 'UserGradeCode', '사용자의 시스템 접근 등급', 'INTEGER', '1', '0,1,2,3', '관리자,데이터관리자,데이터분석가,일반사용자', '0=시스템관리자, 1=데이터관리자, 2=데이터분석가, 3=일반사용자', NOW(), 'system'),
(4, '메뉴관리팀', '메뉴레벨코드', 'MENU_LVL', 'MenuLevelCode', '메뉴의 계층 구조 레벨', 'INTEGER', '1', '0,1,2,3', '최상위,1레벨,2레벨,3레벨', '0=최상위메뉴, 1=1차하위메뉴, 2=2차하위메뉴, 3=3차하위메뉴', NOW(), 'system'),
(5, '시스템관리팀', '데이터타입코드', 'DATA_TYP', 'DataTypeCode', '데이터베이스 컬럼의 데이터 타입', 'VARCHAR', '20', 'VARCHAR,CHAR,INTEGER,TIMESTAMP', '가변문자,고정문자,정수,일시', 'VARCHAR=가변길이문자열, CHAR=고정길이문자열, INTEGER=정수, TIMESTAMP=날짜시간', NOW(), 'system'),
(6, '권한관리팀', '권한유형코드', 'AUTH_TYP', 'AuthTypeCode', '시스템 권한의 유형 분류', 'VARCHAR', '10', 'CREATE,READ,UPDATE,DELETE', '생성,조회,수정,삭제', 'CREATE=생성권한, READ=조회권한, UPDATE=수정권한, DELETE=삭제권한', NOW(), 'system'),
(7, '시스템관리팀', '로그인결과코드', 'LOGIN_RST', 'LoginResultCode', '로그인 시도의 결과 상태', 'CHAR', '1', 'S,F', '성공,실패', 'S=로그인성공, F=로그인실패', NOW(), 'system'),
(8, '데이터관리팀', '암호화여부코드', 'ENCPT_YN', 'EncryptYnCode', '개인정보 암호화 적용 여부', 'CHAR', '1', 'Y,N', '암호화,평문', 'Y=암호화적용, N=평문저장', NOW(), 'system'),
(9, '데이터관리팀', '공개여부코드', 'OPEN_YN', 'OpenYnCode', '데이터의 공개 범위 구분', 'CHAR', '1', 'Y,N', '공개,비공개', 'Y=일반공개, N=제한공개', NOW(), 'system'),
(10, '시스템관리팀', '브라우저타입코드', 'BRWSR_TYP', 'BrowserTypeCode', '접속한 웹브라우저의 종류', 'VARCHAR', '20', 'Chrome,Firefox,Safari,Edge', '크롬,파이어폭스,사파리,엣지', 'Chrome=구글크롬, Firefox=모질라파이어폭스, Safari=애플사파리, Edge=마이크로소프트엣지', NOW(), 'system')
ON CONFLICT (code_korn_nm) DO NOTHING;
```

## 6. 시퀀스 생성 및 조정

```sql
-- ========================================
-- 시퀀스 생성 및 조정
-- ========================================

-- 표준용어사전 시퀀스 생성
CREATE SEQUENCE IF NOT EXISTS "usc_pt".seq_tb_dg_std_trm_list_sn_no
    START WITH 11
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

-- 표준단어사전 시퀀스 생성
CREATE SEQUENCE IF NOT EXISTS "usc_pt".seq_tb_dg_std_word_list_sn_no
    START WITH 11
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

-- 표준도메인사전 시퀀스 생성
CREATE SEQUENCE IF NOT EXISTS "usc_pt".seq_tb_dg_std_dmn_list_sn_no
    START WITH 11
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

-- 표준코드사전 시퀀스 생성
CREATE SEQUENCE IF NOT EXISTS "usc_pt".seq_tb_dg_std_code_list_sn_no
    START WITH 11
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;
```

## 7. 조회용 뷰 생성

```sql
-- ========================================
-- 조회용 뷰 생성
-- ========================================

-- 표준용어사전 조회 뷰
CREATE OR REPLACE VIEW "usc_pt".v_std_term_summary AS
SELECT 
    sn_no,
    trm_korn_nm,
    trm_eng_nm,
    trm_eng_abbr_nm,
    dmn_nm,
    trm_expln_cn,
    jrsd_inst_nm,
    reg_dt,
    rgtr_nm
FROM "usc_pt".tb_dg_std_trm_list
ORDER BY sn_no;

-- 표준단어사전 조회 뷰
CREATE OR REPLACE VIEW "usc_pt".v_std_word_summary AS
SELECT 
    sn_no,
    word_korn_nm,
    word_eng_nm,
    word_eng_abbr_nm,
    word_expln_cn,
    frm_word_yn,
    dmn_clsf_nm,
    reg_dt,
    rgtr_nm
FROM "usc_pt".tb_dg_std_word_list
ORDER BY sn_no;

-- 표준도메인사전 조회 뷰
CREATE OR REPLACE VIEW "usc_pt".v_std_domain_summary AS
SELECT 
    sn_no,
    dmn_nm,
    dmn_group_nm,
    data_typ_nm,
    data_len_cnt,
    dcpt_len_cnt,
    dmn_expln_cn,
    unit_nm,
    prm_vl_nu,
    reg_dt,
    rgtr_nm
FROM "usc_pt".tb_dg_std_dmn_list
ORDER BY dmn_group_nm, sn_no;

-- 표준코드사전 조회 뷰
CREATE OR REPLACE VIEW "usc_pt".v_std_code_summary AS
SELECT 
    sn_no,
    code_korn_nm,
    code_eng_nm,
    code_eng_abbr_nm,
    mng_dept_nm,
    data_typ_nm,
    code_vl_cn,
    cd_vl_mn_cn,
    code_expln_cn,
    reg_dt,
    rgtr_nm
FROM "usc_pt".tb_dg_std_code_list
ORDER BY mng_dept_nm, sn_no;
```

## 8. 데이터 검증 함수

```sql
-- ========================================
-- 데이터 검증 함수
-- ========================================

-- 표준사전 데이터 통계 조회 함수
CREATE OR REPLACE FUNCTION "usc_pt".fn_get_std_dict_stats()
RETURNS TABLE(
    dict_type TEXT,
    total_count BIGINT,
    active_count BIGINT,
    last_updated TIMESTAMP
) AS $$
BEGIN
    -- 표준용어사전 통계
    RETURN QUERY
    SELECT 
        '표준용어사전'::TEXT as dict_type,
        COUNT(*)::BIGINT as total_count,
        COUNT(*)::BIGINT as active_count,
        MAX(reg_dt) as last_updated
    FROM "usc_pt".tb_dg_std_trm_list;
    
    -- 표준단어사전 통계
    RETURN QUERY
    SELECT 
        '표준단어사전'::TEXT as dict_type,
        COUNT(*)::BIGINT as total_count,
        COUNT(*)::BIGINT as active_count,
        MAX(reg_dt) as last_updated
    FROM "usc_pt".tb_dg_std_word_list;
    
    -- 표준도메인사전 통계
    RETURN QUERY
    SELECT 
        '표준도메인사전'::TEXT as dict_type,
        COUNT(*)::BIGINT as total_count,
        COUNT(*)::BIGINT as active_count,
        MAX(reg_dt) as last_updated
    FROM "usc_pt".tb_dg_std_dmn_list;
    
    -- 표준코드사전 통계
    RETURN QUERY
    SELECT 
        '표준코드사전'::TEXT as dict_type,
        COUNT(*)::BIGINT as total_count,
        COUNT(*)::BIGINT as active_count,
        MAX(reg_dt) as last_updated
    FROM "usc_pt".tb_dg_std_code_list;
END;
$$ LANGUAGE plpgsql;

-- 중복 데이터 검증 함수
CREATE OR REPLACE FUNCTION "usc_pt".fn_check_duplicate_names()
RETURNS TABLE(
    dict_type TEXT,
    duplicate_name TEXT,
    duplicate_count BIGINT
) AS $$
BEGIN
    -- 표준용어사전 중복 체크
    RETURN QUERY
    SELECT 
        '표준용어사전'::TEXT as dict_type,
        trm_korn_nm as duplicate_name,
        COUNT(*)::BIGINT as duplicate_count
    FROM "usc_pt".tb_dg_std_trm_list
    GROUP BY trm_korn_nm
    HAVING COUNT(*) > 1;
    
    -- 표준단어사전 중복 체크
    RETURN QUERY
    SELECT 
        '표준단어사전'::TEXT as dict_type,
        word_korn_nm as duplicate_name,
        COUNT(*)::BIGINT as duplicate_count
    FROM "usc_pt".tb_dg_std_word_list
    GROUP BY word_korn_nm
    HAVING COUNT(*) > 1;
    
    -- 표준도메인사전 중복 체크
    RETURN QUERY
    SELECT 
        '표준도메인사전'::TEXT as dict_type,
        dmn_nm as duplicate_name,
        COUNT(*)::BIGINT as duplicate_count
    FROM "usc_pt".tb_dg_std_dmn_list
    GROUP BY dmn_nm
    HAVING COUNT(*) > 1;
    
    -- 표준코드사전 중복 체크
    RETURN QUERY
    SELECT 
        '표준코드사전'::TEXT as dict_type,
        code_korn_nm as duplicate_name,
        COUNT(*)::BIGINT as duplicate_count
    FROM "usc_pt".tb_dg_std_code_list
    GROUP BY code_korn_nm
    HAVING COUNT(*) > 1;
END;
$$ LANGUAGE plpgsql;
```

## 9. 사용 예시

```sql
-- ========================================
-- 사용 예시
-- ========================================

-- 표준사전 통계 조회
SELECT * FROM "usc_pt".fn_get_std_dict_stats();

-- 중복 데이터 확인
SELECT * FROM "usc_pt".fn_check_duplicate_names();

-- 특정 도메인과 관련된 용어 검색
SELECT t.trm_korn_nm, t.trm_eng_nm, t.trm_expln_cn
FROM "usc_pt".tb_dg_std_trm_list t
WHERE t.dmn_nm LIKE '%사용자%';

-- 특정 데이터 타입의 도메인 조회
SELECT d.dmn_nm, d.data_typ_nm, d.data_len_cnt, d.dmn_expln_cn
FROM "usc_pt".tb_dg_std_dmn_list d
WHERE d.data_typ_nm = 'VARCHAR'
ORDER BY d.data_len_cnt;

-- 특정 관리부서의 코드 조회
SELECT c.code_korn_nm, c.code_eng_nm, c.code_vl_cn, c.cd_vl_mn_cn
FROM "usc_pt".tb_dg_std_code_list c
WHERE c.mng_dept_nm = '시스템관리팀'
ORDER BY c.sn_no;
```

## 10. DDL 실행 완료 메시지

```sql
-- ========================================
-- DDL 실행 완료
-- ========================================
DO $$
BEGIN
    RAISE NOTICE '========================================';
    RAISE NOTICE '표준 사전 테이블 DDL 실행이 완료되었습니다.';
    RAISE NOTICE '- 표준용어사전 (TB_DG_STD_TRM_LIST)';
    RAISE NOTICE '- 표준단어사전 (TB_DG_STD_WORD_LIST)';
    RAISE NOTICE '- 표준도메인사전 (TB_DG_STD_DMN_LIST)';
    RAISE NOTICE '- 표준코드사전 (TB_DG_STD_CODE_LIST)';
    RAISE NOTICE '========================================';
    RAISE NOTICE '각 테이블에 10개씩의 임시 데이터가 삽입되었습니다.';
    RAISE NOTICE '========================================';
END $$;
```

---

## 주요 특징

1. **표준화된 구조**: 모든 사전 테이블이 일관된 컬럼 구조를 가짐
2. **확장성**: 새로운 용어, 단어, 도메인, 코드를 쉽게 추가 가능
3. **데이터 무결성**: Primary Key, Unique 제약조건으로 중복 방지
4. **검색 최적화**: 주요 검색 컬럼에 인덱스 생성
5. **유지보수**: 등록자, 수정자, 일시 정보로 변경 이력 추적
6. **실용적 샘플**: 실제 시스템에서 사용할 수 있는 현실적인 임시 데이터

이 DDL을 실행하면 표준 사전 관리를 위한 완전한 데이터베이스 구조가 생성됩니다.


---
## Related
- [[의성마늘 프로그램 설계서]]
- [[2025-09-04 의성마늘 프로젝트 회의록]]
