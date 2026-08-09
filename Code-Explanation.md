# Code Explanation for MindSpace Project

This document provides a comprehensive explanation of the technical architecture of MindSpace, a Flask-based analytics platform for student burnout. It covers the frontend architecture, server-state management, data processing pipeline, machine learning integration, and multi-dataset comparison logic.

## 1. Project Overview

MindSpace is designed to bridge the gap between categorical student metrics (sleep, study, stress) and qualitative well-being (feedback sentiment). The system processes data through four primary lenses:

1. **Algorithmic Scoring**: Deterministic burnout score calculation.
2. **Sentiment Analysis**: NLP-based emotion detection in feedback.
3. **Predictive Modeling**: Supervised learning to identify risk patterns.
4. **Comparative Analytics**: Side-by-side cohort delta analysis.

The application uses a React and TypeScript frontend with React Query for server-state management, while the Flask-based backend handles application logic, data processing, and analytics operations.

## 2. Core Libraries

### Flask (Web Framework)

Flask manages the application lifecycle, routing, and session state. Key implementations include:

* **Jinja2 Templating**: Utilizes template inheritance (`base.html`) for a unified sidebar and theme-toggle experience.
* **Session Handling**: Re-uses `flask.session` to track upload history and metadata without requiring a permanent database.
* **Dynamic Routing**: Maps complex logic for `/dashboard`, `/evaluate`, `/compare`, and `/results`.

### React & TypeScript (Frontend)

The frontend uses React with TypeScript to provide the application's interactive user interface.

The frontend is organized into pages, API functions, and reusable hooks. Recent development has migrated page-level data fetching and mutation handling to React Query, reducing the need to manage server-state manually inside individual components.

Key frontend areas include:

* `frontend/src/App.tsx` — Main React application.
* `frontend/src/pages/` — Application pages such as Dashboard, Home, Edit, and Anomalies.
* `frontend/src/api/` — Functions responsible for communicating with backend API endpoints.
* `frontend/src/hooks/` — Reusable React Query hooks for data fetching and mutations.

### React Query (Server-State Management)

React Query is used to manage asynchronous server data, caching, mutations, and synchronization between the frontend and backend.

The main implementation is located in:

* `frontend/src/hooks/useUpload.ts`
* `frontend/src/api/upload.ts`

The `useUpload.ts` hook provides three main operations:

* **`useHistory()`** — Fetches history data using React Query's `useQuery`.
* **`useUploadFile()`** — Handles dataset uploads using `useMutation`.
* **`useResetSession()`** — Handles session-reset operations using `useMutation`.

#### History Query

The history data is identified using the React Query key `['history']`.

```typescript
export const useHistory = () => {
  return useQuery<HistoryResponse, Error>({
    queryKey: ['history'],
    queryFn: fetchHistory,
  });
};
```

This allows React Query to manage the history data as server state.

#### Upload Mutation

File uploads are handled through the `useUploadFile()` mutation.

```typescript
export const useUploadFile = () => {
  const queryClient = useQueryClient();

  return useMutation<UploadResponse, Error, File>({
    mutationFn: uploadFile,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['history'] });
    },
  });
};
```

After a successful upload, the `['history']` query is invalidated. This tells React Query that the cached history data may no longer represent the current server state, allowing the latest history data to be retrieved.

#### Session Reset Mutation

Session reset operations follow the same pattern:

```typescript
export const useResetSession = () => {
  const queryClient = useQueryClient();

  return useMutation<SuccessResponse, Error, void>({
    mutationFn: resetSession,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['history'] });
    },
  });
};
```

After a successful reset, the same `['history']` query is invalidated so that history-related data remains synchronized with the updated session state.

### React Query Data Flow

The frontend data flow can be summarized as:

```text
React Page
    │
    ▼
React Query Hook
    │
    ├── useHistory()
    │       └── fetchHistory()
    │
    ├── useUploadFile()
    │       └── uploadFile()
    │
    └── useResetSession()
            └── resetSession()
                    │
                    ▼
              Successful Mutation
                    │
                    ▼
       invalidateQueries(['history'])
                    │
                    ▼
            Updated History Data
```

This approach separates API operations from UI components and provides a reusable mechanism for keeping server-derived data synchronized.

### Scikit-Learn (Machine Learning)

The platform features an automated ML pipeline:

* **Random Forest Classifier**: Chosen for its robustness against small datasets and ability to handle non-linear relationships between stress and sleep.
* **Auto-Training**: The `_auto_train()` function triggers on every data change, ensuring the models in the `models/` directory are always synchronized with the visible data.
* **Performance Metrics**: Calculates Accuracy, F1-Score, Precision, and Recall using `sklearn.metrics`, providing a "Deployment Readiness" verdict based on F1-thresholds.

### NLTK (VADER Sentiment)

* **SentimentIntensityAnalyzer**: Processes raw text feedback to produce a "compound score" (-1 to +1).
* **Integration**: These scores are used to identify "Maskers"—students whose numeric metrics look healthy but whose language suggests high distress.

### Pandas & NumPy

* **Fuzzy Column Mapping**: A robust mapping system handles variations in CSV headers (e.g., "sleep_hours" vs "Sleep Hours"), making the app resilient to different data sources.
* **Data Vectorization**: Used for rapid calculation of the Burnout Score across thousands of rows simultaneously.

## 3. The Analytics Pipeline

### Burnout Calculation

The burnout score is calculated as a weighted ratio:

`Score = (Study Hours / Sleep Hours) * Stress Level * 10`

The result is clamped between 0 and 100 to ensure visualization stability.

### The Dashboard

The dashboard uses **Matplotlib (Agg backend)** and **Seaborn** to generate 10 distinct plots:

* **Histograms**: Score distribution.
* **Boxplots**: Comparison of score variance by risk tier.
* **Heatmaps**: Feature correlation.
* **Scatter Plots**: Sentiment vs. Burnout relationships.

### Comparison Logic

The `/compare` module handles two concurrent DataFrames. It syncs them by calculating shared metrics and generating "Delta" badges (e.g., +15% burnout) to highlight differences between two cohorts or time periods.

## 4. Folder Structure & Organization

The project separates frontend application code from backend analytics and processing logic.

```text
MindSpace/
├── frontend/
│   ├── package.json             # Frontend dependencies and scripts
│   ├── package-lock.json        # Locked frontend dependency versions
│   └── src/
│       ├── App.tsx              # Main React application
│       ├── api/
│       │   └── upload.ts        # Upload, history, and session API operations
│       ├── hooks/
│       │   └── useUpload.ts     # React Query queries and mutations
│       └── pages/
│           ├── Dashboard.tsx    # Analytics dashboard
│           ├── Edit.tsx         # Data editing page
│           ├── Home.tsx         # Home/upload page
│           └── Anomalies.tsx    # Anomaly analysis page
│
├── app.py                       # Flask routes, data processing, and backend logic
├── requirements.txt             # Backend dependencies
├── models/                      # Saved model artifacts
├── scripts/                     # Standalone ML scripts
├── templates/                   # Flask/Jinja templates where applicable
└── README.md                    # Project documentation
```

## 5. Frontend Data Management

The frontend uses TanStack React Query to manage server-side data fetching, mutations, caching, and synchronization.

* **React Query Migration**

Several frontend pages were migrated from direct API/data-fetching logic to React Query. This centralizes asynchronous state management and provides consistent loading, error, caching, and refetch behavior across the application.

* **The migration includes:**

frontend/src/hooks/useUpload.ts: Provides reusable React Query hooks for history fetching, file uploads, and session reset operations.
frontend/src/api/upload.ts: Contains the API functions used by the upload-related hooks.
frontend/src/App.tsx and multiple page components: Updated to consume React Query-managed data.
frontend/package.json and frontend/package-lock.json: Updated to support the React Query implementation.
Query and Mutation Handling

The useHistory hook uses useQuery to retrieve upload history:

The history request is managed as a cached query using the history query key.
Components can consume the query's loading, error, and data states without manually managing request state.

* **Mutations are handled through useMutation:**

useUploadFile manages file-upload operations.
useResetSession manages session-reset operations.
Both mutations use useQueryClient() to access the React Query cache.
After a successful upload or session reset, invalidateQueries({ queryKey: ['history'] }) marks the history query as stale so the UI can retrieve the latest history.

This approach keeps the frontend synchronized with backend changes without requiring individual pages to manually refresh or duplicate cache-management logic.

* **Loading and Error States**

The frontend also includes reusable components for communicating asynchronous states to users:

frontend/src/components/Spinner/Spinner.tsx: Provides a reusable loading indicator.
frontend/src/components/Banner/ErrorBanner.tsx: Provides a consistent error message presentation.

These components are used across relevant pages to improve feedback during API operations and make loading and failure states more predictable.

## 6. UI/UX Philosophy

MindSpace prioritizes "Premium Aesthetics":

* **Glassmorphism**: Uses `backdrop-filter: blur` and translucent backgrounds for a state-of-the-art feel.
* **Micro-interactions**: Hover effects on cards, floating action buttons (FABs), and smooth accordion transitions.
* **Visual Synthesis**: Plain-English "Key Takeaways" accompany every chart to ensure findings are accessible to non-technical users.

The frontend architecture also separates reusable data-management logic from presentation components. React Query hooks centralize asynchronous operations such as history retrieval, file uploads, and session resets, allowing individual pages to consume this functionality without duplicating API and cache-management logic.
