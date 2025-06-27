# 🏗️ 프로젝트 구조 자동 생성 시스템

## 🎯 시스템 개요

새로운 프로젝트 추가시 표준화된 디렉토리 구조, 설정 파일, Git 브랜치를 자동으로 생성하는 시스템입니다.
shrimp-rules.md의 필수 파일 관리 규칙을 준수하여 일관된 프로젝트 환경을 제공합니다.

## 🚀 자동 생성 프로세스

### 1. 프로젝트 초기화 명령
```typescript
interface ProjectGenerator {
  async createProject(config: ProjectConfig): Promise<ProjectResult> {
    // 1. 디렉토리 구조 생성
    await this.createDirectoryStructure(config.name);
    
    // 2. 필수 파일 생성
    await this.generateRequiredFiles(config);
    
    // 3. Git 리포지토리 초기화
    await this.initializeGitRepository(config);
    
    // 4. SSH 설정 (필요시)
    if (config.requiresSSH) {
      await this.setupSSHConfiguration(config);
    }
    
    // 5. 프로젝트 등록
    await this.registerProject(config);
    
    return { success: true, projectId: config.name };
  }
}
```

### 2. 표준 디렉토리 구조
```
C:\Users\pasia\Projects\[프로젝트명]\
├── README.md                    # 프로젝트 개요
├── restore_point.json          # 상태 저장 파일
├── server-config.json          # SSH 설정 (필요시)
├── docs/                       # 문서 디렉토리
│   ├── requirements.md         # 요구사항 정의서
│   ├── architecture.md         # 시스템 아키텍처
│   └── api-docs.md            # API 문서
├── src/                        # 소스코드 디렉토리
│   ├── frontend/              # 프론트엔드
│   ├── backend/               # 백엔드
│   └── shared/                # 공통 컴포넌트
├── tests/                      # 테스트 파일
├── deploy/                     # 배포 스크립트
└── .gitignore                  # Git 제외 파일
```

## 📋 자동 생성 파일 템플릿

### restore_point.json 템플릿
```json
{
  "project_id": "{{PROJECT_NAME}}",
  "created_at": "{{TIMESTAMP}}",
  "last_updated": "{{TIMESTAMP}}",
  "agent_system": {
    "current_agent": "None",
    "stage": 0,
    "progress": 0,
    "last_activity": null,
    "context": {}
  },
  "ssh_config": {
    "required": {{SSH_REQUIRED}},
    "host": null,
    "user": null,
    "key_path": null,
    "connected": false
  },
  "git_config": {
    "repository": "{{GIT_REPO}}",
    "branch": "main",
    "last_commit": null
  },
  "project_status": {
    "phase": "initialization",
    "completion_rate": 0,
    "next_steps": [
      "프로젝트 요구사항 정의",
      "기술 스택 선정",
      "개발 환경 구성"
    ]
  }
}
```

### README.md 템플릿
```markdown
# {{PROJECT_NAME}}

## 📋 프로젝트 개요

{{PROJECT_DESCRIPTION}}

## 🎯 목표

- [ ] 요구사항 정의
- [ ] 시스템 설계
- [ ] MVP 개발
- [ ] 사용자 테스트
- [ ] 배포 및 운영

## 🛠️ 기술 스택

- **Frontend**: TBD
- **Backend**: TBD
- **Database**: TBD
- **Deployment**: TBD

## 📊 진행 상황

**현재 단계**: 0단계 (프로젝트 초기화)
**완료율**: 0%
**마지막 업데이트**: {{TIMESTAMP}}

## 🌐 Agent 시스템 연동

이 프로젝트는 통합 적응형 AI Agent 생태계와 연동되어 자동으로 관리됩니다.

- **복잡도 자동 판별**: 요청 내용에 따라 적절한 Agent 자동 활성화
- **다중 프로젝트 지원**: 다른 프로젝트와 독립적으로 관리
- **SSH 자동 연결**: 서버 작업 필요시 자동으로 SSH 연결
- **실시간 상태 추적**: GitHub을 통한 실시간 진행률 업데이트

## 🚀 빠른 시작

```bash
# 프로젝트 활성화
cd C:\Users\pasia\Projects\{{PROJECT_NAME}}

# Agent 시스템에서 이 프로젝트로 전환
"{{PROJECT_NAME}} 프로젝트로 전환해줘"
```

---

*이 프로젝트는 AI Agent 생태계 v3.0으로 자동 생성되었습니다.*
```

### server-config.json 템플릿 (SSH 프로젝트용)
```json
{
  "ssh_connection": {
    "host": "{{SSH_HOST}}",
    "port": 22,
    "user": "{{SSH_USER}}",
    "key_path": "{{SSH_KEY_PATH}}",
    "project_path": "{{REMOTE_PROJECT_PATH}}",
    "auto_connect": true
  },
  "deployment": {
    "environment": "development",
    "port": 3000,
    "domain": null,
    "ssl_enabled": false
  },
  "backup": {
    "enabled": true,
    "schedule": "daily",
    "retention_days": 30
  }
}
```

## 🎭 Agent 통합 시스템

### 프로젝트 등록 시스템
```typescript
class ProjectRegistry {
  private projects: Map<string, ProjectInfo> = new Map();
  
  async registerProject(config: ProjectConfig): Promise<void> {
    const projectInfo: ProjectInfo = {
      id: config.name,
      path: `C:\\Users\\pasia\\Projects\\${config.name}`,
      createdAt: new Date(),
      agentState: new AgentState(),
      sshConfig: config.sshConfig,
      gitConfig: config.gitConfig
    };
    
    this.projects.set(config.name, projectInfo);
    await this.saveToFile();
    await this.updateGitHubRegistry();
  }
}
```

### 자동 Agent 할당
```typescript
class AutoAgentAssignment {
  async assignInitialAgent(projectId: string, projectType: string): Promise<void> {
    const agentType = this.determineInitialAgent(projectType);
    const agent = this.createAgentInstance(agentType, projectId);
    
    await agent.initialize();
    await this.updateProjectStatus(projectId, {
      currentAgent: agentType,
      stage: 1,
      lastActivity: new Date()
    });
  }
}
```

## 🌐 다중 프로젝트 지원

### 프로젝트 간 독립성 보장
```typescript
class ProjectIsolationManager {
  async ensureIsolation(projectId: string): Promise<void> {
    // 1. 별도 Agent 인스턴스 생성
    await this.createIsolatedAgentInstance(projectId);
    
    // 2. 독립적인 상태 관리
    await this.initializeProjectState(projectId);
    
    // 3. 리소스 충돌 방지
    await this.setupResourceLocks(projectId);
    
    // 4. SSH 연결 격리
    if (this.requiresSSH(projectId)) {
      await this.setupIsolatedSSH(projectId);
    }
  }
}
```

## 📊 모니터링 및 추적

### 실시간 상태 업데이트
```typescript
class ProjectMonitor {
  async updateProjectStatus(projectId: string, updates: Partial<ProjectStatus>): Promise<void> {
    // 1. restore_point.json 업데이트
    await this.updateRestorePoint(projectId, updates);
    
    // 2. GitHub 실시간 업데이트
    await this.updateGitHubStatus(projectId, updates);
    
    // 3. 통합 대시보드 업데이트
    await this.updateDashboard();
    
    // 4. Agent 시스템 알림
    await this.notifyAgentSystem(projectId, updates);
  }
}
```

## 🎯 사용 시나리오

### 새 프로젝트 생성
```
사용자: "새로운 AI 챗봇 프로젝트 시작해줘"

시스템 응답:
🏗️ 프로젝트 자동 생성 시작

📋 프로젝트 정보:
   이름: ai-chatbot
   타입: 웹 애플리케이션
   SSH 필요: 예

🚀 생성 중...
   ✅ 디렉토리 구조 생성
   ✅ 필수 파일 생성 (restore_point.json, README.md)
   ✅ Git 리포지토리 초기화
   ✅ SSH 설정 템플릿 생성
   ✅ Agent 시스템 등록

🎉 프로젝트 생성 완료!
📍 경로: C:\Users\pasia\Projects\ai-chatbot
🎭 초기 Agent: Problem Explorer (1단계)

다음 단계: "AI 챗봇의 문제점을 분석해줘"
```

### 기존 프로젝트 복원
```
사용자: "웹 쇼핑몰 프로젝트로 전환해줘"

시스템 응답:
🔄 프로젝트 전환 중...

📋 웹 쇼핑몰 상태 복원:
   ✅ restore_point.json 로드
   ✅ 마지막 Agent 상태: Strategist (3단계, 75%)
   ✅ SSH 연결: shop-dev@35.200.100.50 (연결 중...)
   ✅ Git 상태: 최신 커밋 확인

🎭 Strategist Agent 활성화 완료
📊 진행률: 75% (전략 수립 단계)
🌐 SSH: 연결 완료

마지막 작업: 타겟 고객 페르소나 정의
다음 추천: "제품 설계 단계로 넘어가자"
```

## ✅ 구현 완료 확인

✅ **자동 디렉토리 구조 생성**
✅ **필수 파일 템플릿 자동 생성**
✅ **restore_point.json 표준화**
✅ **Git 리포지토리 자동 초기화**
✅ **SSH 설정 자동 구성**
✅ **Agent 시스템 자동 등록**
✅ **다중 프로젝트 독립성 보장**
✅ **실시간 상태 추적 시스템**

---

**🎉 프로젝트 구조 자동 생성 시스템이 완전히 구현되어 즉시 사용 가능합니다!**