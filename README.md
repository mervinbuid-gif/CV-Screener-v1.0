# CV-Screener (General Workflow) Take Note: each Open Position has its own workflow
{
  "name": "CV Screener - (SR) SSE - Back End",
  "nodes": [
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyMinute"
            }
          ]
        },
        "triggerOn": "specificFolder",
        "folderToWatch": {
          "__rl": true,
          "value": "1TkaYlzz2FUNLkhvFWaBgkGqBiT8juodj",
          "mode": "list",
          "cachedResultName": "SSE - Back End",
          "cachedResultUrl": "https://drive.google.com/drive/folders/1TkaYlzz2FUNLkhvFWaBgkGqBiT8juodj"
        },
        "event": "fileCreated",
        "options": {}
      },
      "id": "8eff98fc-51ec-425f-b0e8-9f70afb7daf2",
      "name": "01 · Drive Trigger",
      "type": "n8n-nodes-base.googleDriveTrigger",
      "typeVersion": 1,
      "position": [
        5616,
        464
      ],
      "credentials": {
        "googleDriveOAuth2Api": {
          "id": "gdqNkqwnmXkFbw2Y",
          "name": "Google Drive account"
        }
      }
    },
    {
      "parameters": {
        "operation": "download",
        "fileId": {
          "__rl": true,
          "value": "={{ $json.id }}",
          "mode": "id"
        },
        "options": {}
      },
      "id": "a38a1235-f14a-4a27-8694-2bf2b2a1e781",
      "name": "02 · Download CV",
      "type": "n8n-nodes-base.googleDrive",
      "typeVersion": 3,
      "position": [
        5840,
        464
      ],
      "credentials": {
        "googleDriveOAuth2Api": {
          "id": "gdqNkqwnmXkFbw2Y",
          "name": "Google Drive account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "// ═══════════════════════════════════════════════════════════════\n// NODE 03 (v2): Build Claude Request — stricter scoring\n//\n// MODE: \"Run Once for All Items\"\n//\n// Role: Senior Software Engineer (Backend – Java Spring Boot)\n// ═══════════════════════════════════════════════════════════════\n\nconst driveData = $('01 · Drive Trigger').first().json;\nconst fileName = driveData.name || 'unknown';\nconst fileLink = driveData.webViewLink || '';\n\nconst extractedData = $input.first().json;\nconst cvText = extractedData.data || extractedData.text || extractedData.content || '';\n\nif (!cvText || cvText.length < 50) {\n  return [{ json: {\n    error: 'Text extraction produced no/insufficient content. Length: ' + cvText.length,\n    file_name: fileName,\n    file_link: fileLink,\n    request_body: '{}'\n  }}];\n}\n\nconst systemPrompt = `You are a strict, calibrated senior technical recruiter AI for AirAsia MOVE. Screen CVs against the job description below. You are deliberately conservative in scoring.\n\n## CALIBRATION RULES (READ FIRST)\n\nYou must follow these calibration rules strictly:\n\n1. **Evidence-only scoring**: Only award points for skills/experience EXPLICITLY stated with concrete details (project names, technologies, metrics, system scale). Vague claims like \"built microservices\" without specifics = half points at most.\n\n2. **Job title ≠ evidence**: A title like \"Senior Backend Engineer\" does not automatically prove depth. Look for described projects, Spring ecosystem components used hands-on, architecture decisions, and measurable outcomes.\n\n3. **Recency matters**: Weight recent experience (last 3-5 years) more heavily. Skills from 8+ years ago with no recent evidence get reduced scores.\n\n4. **Score distribution targets**: \n   - A typical strong candidate should score 55-70 (YELLOW)\n   - Only exceptional candidates with deep evidence across ALL criteria score 75+ (GREEN)  \n   - A score above 85 should be extremely rare — reserved for candidates who match nearly every criterion with strong evidence\n   - 90+ is virtually impossible — it means perfect match on every dimension\n\n5. **Java Spring Boot proficiency requires EXPLICIT evidence**: Mentioning \"Java\" or \"Spring\" is worth 3-4 points. Full marks require evidence of hands-on Spring Boot + Spring ecosystem components (Spring MVC, Spring Data JPA, Spring Security) with specific services built, API designs, and production outcomes.\n\n6. **Microservices scoring**: Simply mentioning \"microservices\" is worth 2-3 points. Full marks require evidence of domain-driven design, specific service decomposition decisions, resilience patterns implemented, and scale metrics.\n\n7. **Deductions**: If the CV is poorly structured, lacks quantifiable achievements, or reads as a task list rather than an impact summary, deduct 1-2 points from collaboration and system design (these require communication skills the CV should demonstrate).\n\n## JOB DESCRIPTION\n**Role:** Senior Software Engineer (Backend – Java Spring Boot)\n**Location:** Kuala Lumpur\n**Company:** AirAsia MOVE (ASEAN-focused budget travel platform, part of Capital A)\n\nWe are looking for a Senior Software Engineer (Backend – Java Spring Boot) to build and scale backend services using the Java ecosystem, powering critical systems across booking, payments, and travel operations. You will work on high-throughput, distributed systems, ensuring reliability, performance, and maintainability in production environments. You are expected to bring strong backend expertise, contribute to system design, and elevate engineering standards within the team.\n\n**Required:** 5+ years of backend software engineering experience. Strong proficiency in Java and Spring Boot framework. Hands-on experience with Spring ecosystem (Spring MVC, Spring Data JPA, Spring Security). Experience building RESTful APIs and microservices architectures. Strong understanding of OOD, design patterns, and system design. Experience with relational databases (MySQL, PostgreSQL) and ORM tools (Hibernate/JPA). Familiarity with message queues (Kafka, RabbitMQ), Git workflows, Maven/Gradle, Docker, and cloud platforms (AWS/GCP/Azure).\n\n## SCORING RUBRIC (100 points total)\n\n### A. MUST-HAVE CRITERIA (70 points)\n\n1. **Backend Engineering Experience Depth** (10 pts)\n   - 8+ years backend-focused with clear progression, diverse projects, and strong fundamentals: 9-10 pts\n   - 5-8 years with good backend breadth (meets JD requirement): 6-8 pts\n   - 3-5 years (below JD minimum): 3-5 pts\n   - <3 years: 0-2 pts\n\n2. **Java & Spring Boot Proficiency** (10 pts)\n   - Deep hands-on Java + Spring Boot with evidence of Spring MVC, Spring Data JPA, Spring Security in production services: 8-10 pts\n   - Strong Java + Spring Boot with some Spring ecosystem specifics: 5-7 pts\n   - Java experience but limited/no Spring Boot evidence: 3-4 pts\n   - No Java evidence: 0-2 pts\n\n3. **RESTful APIs & Microservices Architecture** (10 pts)\n   - Built RESTful APIs + microservices following domain-driven design with specific examples of service decomposition, API contracts, and production outcomes: 8-10 pts\n   - Built APIs and microservices with some details: 5-7 pts\n   - Some API work but monolithic or limited microservices evidence: 2-4 pts\n   - No evidence: 0-1 pt\n\n4. **Database & Data Layer (SQL, ORM, Messaging)** (10 pts)\n   - Hands-on MySQL/PostgreSQL + Hibernate/JPA + message queues (Kafka/RabbitMQ) with specific data modelling and async processing examples: 8-10 pts\n   - Strong DB experience + some ORM/messaging: 5-7 pts\n   - Basic database usage, no ORM or messaging evidence: 2-4 pts\n   - No evidence: 0-1 pt\n\n5. **System Design & OOD Principles** (10 pts)\n   - Evidence of system design for high-traffic distributed systems + SOLID principles + clean architecture + design patterns with specific examples: 8-10 pts\n   - Some system design and OOD evidence: 5-7 pts\n   - Basic design awareness, no concrete examples: 2-4 pts\n   - No evidence: 0-1 pt\n\n6. **Docker, Cloud & DevOps Fundamentals** (10 pts)\n   - Hands-on Docker + cloud platforms (AWS/GCP/Azure) for backend services + Maven/Gradle + Git workflows with specific deployment examples: 8-10 pts\n   - Docker + some cloud with limited specifics: 5-7 pts\n   - Basic cloud or container exposure: 2-4 pts\n   - No evidence: 0-1 pt\n\n7. **Testing, Debugging & Production Operations** (10 pts)\n   - Strong testing practice (JUnit, Mockito) + integration/API tests + production debugging + performance tuning with specific examples: 8-10 pts\n   - Some testing and debugging evidence: 5-7 pts\n   - Basic unit testing only or minimal production ops: 2-4 pts\n   - No evidence: 0-1 pt\n\n### B. PREFERRED CRITERIA (20 points)\n\n8. **Spring Cloud & Distributed System Patterns** (5 pts)\n   - Hands-on Spring Cloud (Config Server, Eureka, Gateway) + distributed patterns (circuit breaker, retries, rate limiting) with specifics: 4-5 pts\n   - Some Spring Cloud or distributed pattern exposure: 2-3 pts\n   - No evidence: 0-1 pt\n\n9. **Caching & Performance Optimisation** (5 pts)\n   - Hands-on caching (Redis, Hazelcast) + performance tuning at scale with metrics and outcomes: 4-5 pts\n   - Some caching or performance work: 2-3 pts\n   - No evidence: 0-1 pt\n\n10. **Observability & Monitoring** (5 pts)\n    - Hands-on with Prometheus, Grafana, ELK stack, or similar + production observability with specific implementations: 4-5 pts\n    - Some monitoring experience: 2-3 pts\n    - No evidence: 0-1 pt\n\n11. **Security Best Practices & High-Scale Systems** (5 pts)\n    - Evidence of OAuth2/JWT/API security implementation + experience with high-scale transaction-heavy systems (payments, bookings): 4-5 pts\n    - Some security or high-scale evidence: 2-3 pts\n    - No evidence: 0-1 pt\n\n### C. BONUS SIGNALS (10 points)\n\n12. **Mentorship & Engineering Culture** (3 pts)\n    - Mentored junior engineers, contributed to backend best practices, code review culture, and internal documentation: 2-3 pts\n    - Some mentoring evidence: 1 pt\n    - No evidence: 0 pts\n\n13. **ASEAN / Travel / E-commerce Domain** (4 pts)\n    - Travel tech or e-commerce in ASEAN with hands-on backend work on booking/payment systems: 3-4 pts\n    - Travel/e-commerce elsewhere or tangential ASEAN: 1-2 pts\n    - No relevance: 0 pts\n\n14. **Agile Practices & Continuous Improvement** (3 pts)\n    - Active Agile/Scrum participation + evidence of driving engineering improvements, adopting new practices: 2-3 pts\n    - Some Agile experience: 1 pt\n    - No evidence: 0 pts\n\n## OUTPUT FORMAT\n\nRespond ONLY in valid JSON. No preamble, no markdown backticks.\n\n{\n  \"candidate_name\": \"Full Name\",\n  \"current_role\": \"Current or most recent role\",\n  \"current_company\": \"Current or most recent company\",\n  \"years_experience\": 0,\n  \"primary_languages\": \"e.g. Java, Spring Boot\",\n  \"scores\": {\n    \"backend_experience\": { \"score\": 0, \"max\": 10, \"evidence\": \"...\" },\n    \"java_spring_boot\": { \"score\": 0, \"max\": 10, \"evidence\": \"...\" },\n    \"api_microservices\": { \"score\": 0, \"max\": 10, \"evidence\": \"...\" },\n    \"database_messaging\": { \"score\": 0, \"max\": 10, \"evidence\": \"...\" },\n    \"system_design_ood\": { \"score\": 0, \"max\": 10, \"evidence\": \"...\" },\n    \"docker_cloud_devops\": { \"score\": 0, \"max\": 10, \"evidence\": \"...\" },\n    \"testing_debugging_ops\": { \"score\": 0, \"max\": 10, \"evidence\": \"...\" },\n    \"spring_cloud_distributed\": { \"score\": 0, \"max\": 5, \"evidence\": \"...\" },\n    \"caching_performance\": { \"score\": 0, \"max\": 5, \"evidence\": \"...\" },\n    \"observability_monitoring\": { \"score\": 0, \"max\": 5, \"evidence\": \"...\" },\n    \"security_highscale\": { \"score\": 0, \"max\": 5, \"evidence\": \"...\" },\n    \"mentorship_culture\": { \"score\": 0, \"max\": 3, \"evidence\": \"...\" },\n    \"asean_travel_ecommerce\": { \"score\": 0, \"max\": 4, \"evidence\": \"...\" },\n    \"agile_improvement\": { \"score\": 0, \"max\": 3, \"evidence\": \"...\" }\n  },\n  \"total_score\": 0,\n  \"flag\": \"GREEN\",\n  \"flag_reasoning\": \"1 sentence\",\n  \"summary\": \"bullet 1|bullet 2|bullet 3\",\n  \"key_skills_matched\": [],\n  \"red_flags\": [],\n  \"recommendation\": \"SHORTLIST\"\n}\n\n## FLAG THRESHOLDS\n- GREEN (75-100): Strong match — prioritize for interview\n- YELLOW (50-74): Partial match — review manually  \n- RED (0-49): Weak match — FAIL\n\n## RECOMMENDATION MAPPING\n- GREEN → \"SHORTLIST\"\n- YELLOW → \"REVIEW\"\n- RED → \"FAIL\"\n\n## SCORE SANITY CHECK (do this before responding)\n- Add up all individual scores. Does the total match total_score? If not, fix it.\n- Is the total above 85? Re-examine each score — did you give full marks without strong evidence? Reduce over-generous scores.\n- Is java_spring_boot above 7? What SPECIFIC Spring ecosystem components are evidenced? Generic \"Java developer\" = 3-4 max.\n- Is api_microservices above 7? Is there evidence of domain-driven design and specific service decomposition, or just \"built APIs\"?\n- Is database_messaging above 7? Is there evidence of BOTH relational DB + messaging, or just one?\n- Remember: summary uses | as separator between the 3 bullets (not newlines).`;\n\nconst requestBody = JSON.stringify({\n  model: 'claude-sonnet-4-20250514',\n  max_tokens: 2000,\n  system: systemPrompt,\n  messages: [\n    {\n      role: 'user',\n      content: 'Screen the following CV against the Senior Software Engineer (Backend – Java Spring Boot) role at AirAsia MOVE. Be strict and evidence-based. Score conservatively.\\n\\nCV CONTENT:\\n' + cvText\n    }\n  ]\n});\n\nreturn [{ json: {\n  request_body: requestBody,\n  file_name: fileName,\n  file_link: fileLink,\n  cv_text_length: cvText.length\n}}];"
      },
      "id": "206a3c41-6007-48cf-aa54-6fab3aeb3987",
      "name": "03 · Build Claude Request",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        6272,
        464
      ],
      "notes": "Converts binary CV to base64 and builds the FULL Claude API request body as a JSON string. NO network calls — safe for sandboxed Code nodes."
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.anthropic.com/v1/messages",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "httpHeaderAuth",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "content-type",
              "value": "application/json"
            },
            {
              "name": "anthropic-version",
              "value": "2023-06-01"
            }
          ]
        },
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={{ JSON.parse($json.request_body) }}",
        "options": {
          "timeout": 120000
        }
      },
      "id": "208bd8c7-7e9e-4e62-9934-854dc6b41cd3",
      "name": "04 · Claude Screen CV",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        6496,
        464
      ],
      "credentials": {
        "httpHeaderAuth": {
          "id": "hc9lZ8KOpwgLU3yj",
          "name": "Header Auth account"
        }
      },
      "notes": "Sends the pre-built request body to Claude API.\n\nBody mode = String (not JSON) so the base64 passes through intact.\nTimeout = 120s for PDF analysis.\n\nCredential setup: HTTP Header Auth\n  Header Name: x-api-key\n  Header Value: your Anthropic API key"
    },
    {
      "parameters": {
        "jsCode": "// Parse Claude's response into flat columns for Google Sheets\n// Role: Senior Software Engineer (Backend – Java Spring Boot)\nconst input = $input.first().json;\n// Get file info from Build Request node\nconst buildData = $('03 · Build Claude Request').first().json;\nconst fileName = buildData.file_name || 'Unknown';\nconst fileLink = buildData.file_link || '';\n// Extract Claude's text response\nlet content = '';\nif (input.content && Array.isArray(input.content)) {\n  content = input.content.map(c => c.text || '').join('');\n} else if (typeof input === 'string') {\n  content = input;\n} else {\n  content = JSON.stringify(input);\n}\nlet parsed;\ntry {\n  const clean = content.replace(/```json|```/g, '').trim();\n  parsed = JSON.parse(clean);\n} catch (e) {\n  return [{ json: {\n    timestamp: new Date().toISOString(),\n    candidate_name: 'PARSE ERROR',\n    current_role: '', current_company: '',\n    years_experience: 0, primary_languages: '',\n    total_score: 0, flag: 'RED', recommendation: 'FAIL',\n    s1_backend_experience: 0,\n    s2_java_spring_boot: 0,\n    s3_api_microservices: 0,\n    s4_database_messaging: 0,\n    s5_system_design_ood: 0,\n    s6_docker_cloud_devops: 0,\n    s7_testing_debugging_ops: 0,\n    s8_spring_cloud_distributed: 0,\n    s9_caching_performance: 0,\n    s10_observability_monitoring: 0,\n    s11_security_highscale: 0,\n    s12_mentorship_culture: 0,\n    s13_asean_travel_ecommerce: 0,\n    s14_agile_improvement: 0,\n    key_skills: '', red_flags: 'Parse failed: ' + content.substring(0, 300),\n    summary: 'Manual review needed', flag_reasoning: 'JSON parse error',\n    cv_link: fileLink, file_name: fileName,\n    flag_emoji: '⚠️', full_evaluation_json: content.substring(0, 500)\n  }}];\n}\nconst flagEmoji = parsed.flag === 'GREEN' ? '🟢' : parsed.flag === 'YELLOW' ? '🟡' : '🔴';\nreturn [{ json: {\n  timestamp: new Date().toISOString(),\n  candidate_name: parsed.candidate_name || 'Unknown',\n  current_role: parsed.current_role || '',\n  current_company: parsed.current_company || '',\n  years_experience: parsed.years_experience || 0,\n  primary_languages: parsed.primary_languages || '',\n  total_score: parsed.total_score || 0,\n  flag: parsed.flag || 'RED',\n  recommendation: parsed.recommendation || 'FAIL',\n  // A. Must-Have (70 pts)\n  s1_backend_experience: parsed.scores?.backend_experience?.score || 0,\n  s2_java_spring_boot: parsed.scores?.java_spring_boot?.score || 0,\n  s3_api_microservices: parsed.scores?.api_microservices?.score || 0,\n  s4_database_messaging: parsed.scores?.database_messaging?.score || 0,\n  s5_system_design_ood: parsed.scores?.system_design_ood?.score || 0,\n  s6_docker_cloud_devops: parsed.scores?.docker_cloud_devops?.score || 0,\n  s7_testing_debugging_ops: parsed.scores?.testing_debugging_ops?.score || 0,\n  // B. Preferred (20 pts)\n  s8_spring_cloud_distributed: parsed.scores?.spring_cloud_distributed?.score || 0,\n  s9_caching_performance: parsed.scores?.caching_performance?.score || 0,\n  s10_observability_monitoring: parsed.scores?.observability_monitoring?.score || 0,\n  s11_security_highscale: parsed.scores?.security_highscale?.score || 0,\n  // C. Bonus (10 pts)\n  s12_mentorship_culture: parsed.scores?.mentorship_culture?.score || 0,\n  s13_asean_travel_ecommerce: parsed.scores?.asean_travel_ecommerce?.score || 0,\n  s14_agile_improvement: parsed.scores?.agile_improvement?.score || 0,\n  key_skills: (parsed.key_skills_matched || []).join(', '),\n  red_flags: (parsed.red_flags || []).join(', '),\n  summary: parsed.summary || '',\n  flag_reasoning: parsed.flag_reasoning || '',\n  cv_link: fileLink,\n  file_name: fileName,\n  flag_emoji: flagEmoji,\n  full_evaluation_json: JSON.stringify(parsed)\n}}];"
      },
      "id": "bcc49763-b49a-47cc-91c7-509b1be862b4",
      "name": "05 · Parse Response",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        6704,
        464
      ],
      "notes": "Parses Claude's JSON into flat columns for Sheets. Error-safe."
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1S2Gq6lTQnYc0ytTBy4OF4ZCAX-CJOs3Gyh7Q7djHxBY",
          "mode": "list",
          "cachedResultName": "CV Screener",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1S2Gq6lTQnYc0ytTBy4OF4ZCAX-CJOs3Gyh7Q7djHxBY/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1634264203,
          "mode": "list",
          "cachedResultName": "CV Screening",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1S2Gq6lTQnYc0ytTBy4OF4ZCAX-CJOs3Gyh7Q7djHxBY/edit#gid=1634264203"
        },
        "columns": {
          "mappingMode": "autoMapInputData",
          "value": {},
          "matchingColumns": [],
          "schema": [
            {
              "id": "Timestamp",
              "displayName": "Timestamp",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Candidate Name",
              "displayName": "Candidate Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Current Role",
              "displayName": "Current Role",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Current Company",
              "displayName": "Current Company",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Yrs Exp",
              "displayName": "Yrs Exp",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Yrs Leadership",
              "displayName": "Yrs Leadership",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Total Score",
              "displayName": "Total Score",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Flag",
              "displayName": "Flag",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Recommendation",
              "displayName": "Recommendation",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Eng Experience /10",
              "displayName": "Eng Experience /10",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Eng Leadership /10",
              "displayName": "Eng Leadership /10",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Platform Leadership /10",
              "displayName": "Platform Leadership /10",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Distributed+Cloud /10",
              "displayName": "Distributed+Cloud /10",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Cloud Infra /10",
              "displayName": "Cloud Infra /10",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Architecture /10",
              "displayName": "Architecture /10",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Stakeholder Mgmt /10",
              "displayName": "Stakeholder Mgmt /10",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Platform Domains /5",
              "displayName": "Platform Domains /5",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Full-Stack /5",
              "displayName": "Full-Stack /5",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "CI/CD DevOps /5",
              "displayName": "CI/CD DevOps /5",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Startup Exp /5",
              "displayName": "Startup Exp /5",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Multi-tenant /3",
              "displayName": "Multi-tenant /3",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "ASEAN Domain /4",
              "displayName": "ASEAN Domain /4",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Team Scaling /3",
              "displayName": "Team Scaling /3",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Key Skills",
              "displayName": "Key Skills",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Red Flags",
              "displayName": "Red Flags",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Summary",
              "displayName": "Summary",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Flag Reasoning",
              "displayName": "Flag Reasoning",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "CV Link",
              "displayName": "CV Link",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "id": "9321161f-42cd-4b6c-b52b-46c217922420",
      "name": "06 · Write to Sheets",
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.5,
      "position": [
        6928,
        464
      ],
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "eyMKinMoOvdqNk0a",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "sendTo": "mervinbuid@airasia.com",
        "subject": "={{ $json.flag_emoji }} CV Screened: {{ $json.candidate_name }} ({{ $json.total_score }}/100) — {{ $json.recommendation }}",
        "message": "=<div style=\"font-family: Arial, sans-serif; max-width: 600px;\">\n  <div style=\"padding: 16px 20px; border-radius: 8px; margin-bottom: 16px; {{ $json.flag === 'GREEN' ? 'background: #d4edda; border-left: 4px solid #28a745;' : $json.flag === 'YELLOW' ? 'background: #fff3cd; border-left: 4px solid #ffc107;' : 'background: #f8d7da; border-left: 4px solid #dc3545;' }}\">\n    <strong style=\"font-size: 16px;\">{{ $json.flag_emoji }} {{ $json.flag }} — {{ $json.recommendation }}</strong>\n  </div>\n  \n  <table style=\"width: 100%; border-collapse: collapse; margin-bottom: 16px;\">\n    <tr><td style=\"padding: 6px 0; color: #666; width: 130px;\">Candidate</td><td style=\"padding: 6px 0;\"><strong>{{ $json.candidate_name }}</strong></td></tr>\n    <tr><td style=\"padding: 6px 0; color: #666;\">Current Role</td><td style=\"padding: 6px 0;\">{{ $json.current_role }}</td></tr>\n    <tr><td style=\"padding: 6px 0; color: #666;\">Company</td><td style=\"padding: 6px 0;\">{{ $json.current_company }}</td></tr>\n    <tr><td style=\"padding: 6px 0; color: #666;\">Experience</td><td style=\"padding: 6px 0;\">{{ $json.years_experience }} yrs · {{ $json.primary_languages }}</td></tr>\n    <tr><td style=\"padding: 6px 0; color: #666;\">Score</td><td style=\"padding: 6px 0;\"><strong style=\"font-size: 18px;\">{{ $json.total_score }}/100</strong></td></tr>\n    <tr><td style=\"padding: 6px 0; color: #666;\">Source File</td><td style=\"padding: 6px 0;\">{{ $json.file_name }}</td></tr>\n  </table>\n\n  <div style=\"margin-bottom: 16px; padding: 14px; background: #f8f9fa; border-radius: 8px;\">\n    <strong style=\"font-size: 13px; color: #555; text-transform: uppercase; letter-spacing: 0.5px;\">Must-Have Criteria ({{ $json.s1_backend_experience + $json.s2_java_spring_boot + $json.s3_api_microservices + $json.s4_database_messaging + $json.s5_system_design_ood + $json.s6_docker_cloud_devops + $json.s7_testing_debugging_ops }}/70)</strong>\n    <table style=\"width: 100%; border-collapse: collapse; margin-top: 8px; font-size: 13px;\">\n      <tr><td style=\"padding: 3px 0; color: #444;\">Backend Experience Depth</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold; width: 50px;\">{{ $json.s1_backend_experience }}/10</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Java & Spring Boot</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s2_java_spring_boot }}/10</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">RESTful APIs & Microservices</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s3_api_microservices }}/10</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Database, ORM & Messaging</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s4_database_messaging }}/10</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">System Design & OOD</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s5_system_design_ood }}/10</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Docker, Cloud & DevOps</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s6_docker_cloud_devops }}/10</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Testing, Debugging & Prod Ops</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s7_testing_debugging_ops }}/10</td></tr>\n    </table>\n  </div>\n\n  <div style=\"margin-bottom: 16px; padding: 14px; background: #f8f9fa; border-radius: 8px;\">\n    <strong style=\"font-size: 13px; color: #555; text-transform: uppercase; letter-spacing: 0.5px;\">Preferred Criteria ({{ $json.s8_spring_cloud_distributed + $json.s9_caching_performance + $json.s10_observability_monitoring + $json.s11_security_highscale }}/20)</strong>\n    <table style=\"width: 100%; border-collapse: collapse; margin-top: 8px; font-size: 13px;\">\n      <tr><td style=\"padding: 3px 0; color: #444;\">Spring Cloud & Distributed Patterns</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold; width: 50px;\">{{ $json.s8_spring_cloud_distributed }}/5</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Caching & Performance</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s9_caching_performance }}/5</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Observability & Monitoring</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s10_observability_monitoring }}/5</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Security & High-Scale Systems</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s11_security_highscale }}/5</td></tr>\n    </table>\n  </div>\n\n  <div style=\"margin-bottom: 16px; padding: 14px; background: #f8f9fa; border-radius: 8px;\">\n    <strong style=\"font-size: 13px; color: #555; text-transform: uppercase; letter-spacing: 0.5px;\">Bonus Signals ({{ $json.s12_mentorship_culture + $json.s13_asean_travel_ecommerce + $json.s14_agile_improvement }}/10)</strong>\n    <table style=\"width: 100%; border-collapse: collapse; margin-top: 8px; font-size: 13px;\">\n      <tr><td style=\"padding: 3px 0; color: #444;\">Mentorship & Engineering Culture</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold; width: 50px;\">{{ $json.s12_mentorship_culture }}/3</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">ASEAN / Travel / E-commerce</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s13_asean_travel_ecommerce }}/4</td></tr>\n      <tr><td style=\"padding: 3px 0; color: #444;\">Agile & Continuous Improvement</td><td style=\"padding: 3px 0; text-align: right; font-weight: bold;\">{{ $json.s14_agile_improvement }}/3</td></tr>\n    </table>\n  </div>\n  \n  <div style=\"margin-bottom: 16px;\">\n    <strong>Key Skills:</strong> {{ $json.key_skills }}\n  </div>\n  \n  <div style=\"margin-bottom: 16px;\">\n    <strong>Summary:</strong><br>{{ $json.summary }}\n  </div>\n  \n  {{ $json.red_flags ? '<div style=\"margin-bottom: 16px; padding: 10px; background: #fff3cd; border-radius: 4px;\"><strong>Flags:</strong> ' + $json.red_flags + '</div>' : '' }}\n  \n  <div style=\"margin-bottom: 16px;\">\n    <strong>Reasoning:</strong> {{ $json.flag_reasoning }}\n  </div>\n  \n  <div style=\"margin-top: 20px; padding-top: 16px; border-top: 1px solid #eee;\">\n    {{ $json.cv_link ? '<a href=\"' + $json.cv_link + '\" style=\"color: #0066cc;\">Open CV in Drive</a> · ' : '' }}<span style=\"color: #999;\">CV Screening Bot · Sr Software Engineer (Backend – Java Spring Boot)</span>\n  </div>\n</div>",
        "options": {}
      },
      "id": "3eb44a0d-6674-47b5-be9a-df03717e3eb8",
      "name": "07 · Notify Mervin",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        7152,
        464
      ],
      "webhookId": "a13e2374-b25c-4ae8-9655-8fc521242adf",
      "credentials": {
        "gmailOAuth2": {
          "id": "RA0rd0n8fm8aenym",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "operation": "pdf",
        "options": {}
      },
      "type": "n8n-nodes-base.extractFromFile",
      "typeVersion": 1.1,
      "position": [
        6048,
        464
      ],
      "id": "81737557-8df6-4a16-8b8a-fc4c656345b1",
      "name": "Extract from File"
    }
  ],
  "pinData": {},
  "connections": {
    "01 · Drive Trigger": {
      "main": [
        [
          {
            "node": "02 · Download CV",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "02 · Download CV": {
      "main": [
        [
          {
            "node": "Extract from File",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "03 · Build Claude Request": {
      "main": [
        [
          {
            "node": "04 · Claude Screen CV",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "04 · Claude Screen CV": {
      "main": [
        [
          {
            "node": "05 · Parse Response",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "05 · Parse Response": {
      "main": [
        [
          {
            "node": "06 · Write to Sheets",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "06 · Write to Sheets": {
      "main": [
        [
          {
            "node": "07 · Notify Mervin",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract from File": {
      "main": [
        [
          {
            "node": "03 · Build Claude Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "availableInMCP": false
  },
  "versionId": "3e0d2794-1029-4b50-a3b7-4815d010a8d1",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "4d473ff90cd7e86015ef665f798f987277cbf68e22d36321bededaaad0e4f986"
  },
  "id": "uRCynlndgpiTtMCi",
  "tags": [
    {
      "updatedAt": "2026-03-16T14:45:23.687Z",
      "createdAt": "2026-03-16T14:45:23.687Z",
      "id": "iId1U6YCzL0omBdY",
      "name": "recruitment"
    },
    {
      "updatedAt": "2026-03-16T14:45:23.737Z",
      "createdAt": "2026-03-16T14:45:23.737Z",
      "id": "8YxNQjK66abCz5Nl",
      "name": "cv-screening"
    },
    {
      "updatedAt": "2026-03-16T14:45:23.739Z",
      "createdAt": "2026-03-16T14:45:23.739Z",
      "id": "jNGVNjY0huk5qWm0",
      "name": "phase-1"
    },
    {
      "updatedAt": "2026-03-16T15:02:14.484Z",
      "createdAt": "2026-03-16T15:02:14.484Z",
      "id": "UIPmhw5AY4q0KPC0",
      "name": "phase-1-v2"
    },
    {
      "updatedAt": "2026-03-16T15:18:44.933Z",
      "createdAt": "2026-03-16T15:18:44.933Z",
      "id": "V6sqkG6cso7eA09c",
      "name": "phase-1-v3"
    },
    {
      "updatedAt": "2026-03-16T15:24:38.624Z",
      "createdAt": "2026-03-16T15:24:38.624Z",
      "id": "K3YNmGCZ4x0OqWUg",
      "name": "phase-1-v4"
    },
    {
      "updatedAt": "2026-03-16T15:56:22.228Z",
      "createdAt": "2026-03-16T15:56:22.228Z",
      "id": "VwCTUnMrTuGZZBR7",
      "name": "phase-1-v7"
    }
  ]
}
