# Процесс управления Helm-репозиториями через Fleet в Rancher:

```mermaid
graph LR
  A[Cluster Management] -->|Создает глобальный Helm-репозиторий| B[Rancher Server]
  B --> E[Fleet]
  E -->|Через GitRepo ресурс| C[Кластер 1]
  E -->|Через GitRepo ресурс| D[Кластер 2]
  
  subgraph GitOps Flow
  direction TB
  G[Git Repository] -->|Конфигурация HelmRepository| E
  end
  
  subgraph Cluster 1
  C --> F1[Helm Repository]
  F1 --> H1[Доступные чарты]
  end
  
  subgraph Cluster 2
  D --> F2[Helm Repository]
  F2 --> H2[Доступные чарты]
  end
  
  style A fill:#f9f,stroke:#333
  style B fill:#e6f7ff,stroke:#333
  style E fill:#cce5ff,stroke:#333
  style G fill:#d4edda,stroke:#333
```

### Пояснение элементов:
1. **Cluster Management** (розовый):  
   - Интерфейс администратора для создания глобальных Helm-репозиториев

2. **Rancher Server** (голубой):  
   - Центральный управляющий сервер
   - Хранит конфигурацию глобальных репозиториев

3. **Fleet** (синий):  
   - Компонент непрерывной доставки
   - Обрабатывает GitRepo-ресурсы
   - Синхронизирует конфигурацию с кластерами

4. **Git Repository** (зеленый):  
   - Содержит конфигурацию HelmRepository в YAML-формате
   - Изменения в Git автоматически триггерят обновления

5. **Кластеры** (серые блоки):  
   - Получают конфигурацию Helm-репозиториев через Fleet
   - Автоматически индексируют чарты для использования

### Ключевые связи:
- `Cluster Management → Rancher Server`: Администратор создает глобальный источник
- `Rancher Server → Fleet`: Активирует механизм распространения
- `Fleet → Кластеры`: Применяет конфигурацию через GitRepo-ресурсы
- `Git Repository → Fleet`: Обеспечивает GitOps-подход (конфигурация как код)
 