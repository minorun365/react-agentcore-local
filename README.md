# react-agentcore-local

React + AgentCore のローカル開発サンプル（認証なし）

## ディレクトリ構成

```
react-agentcore-local/
├── frontend/           # Vite + React
│   ├── src/
│   │   └── App.tsx
│   └── package.json
├── backend/            # Python + AgentCore
│   ├── src/
│   │   └── main.py
│   └── requirements.txt
└── README.md
```

## 起動方法（ターミナル2つ）

### 1. バックエンド

```bash
cd backend
pip install -r requirements.txt
agentcore dev
```

### 2. フロントエンド

```bash
cd frontend
npm install
npm run dev
```

### 3. ブラウザでアクセス

http://localhost:5173


---

## ゼロから作る手順

### 1. プロジェクト作成

```bash
mkdir react-agentcore-local && cd react-agentcore-local
mkdir -p backend/src
```

### 2. フロントエンド

```bash
npm create vite@latest frontend -- --template react-swc-ts
cd frontend
npm install react-markdown
```

`vite.config.ts` にプロキシ設定を追加：

```typescript
export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
    },
  },
})
```

`src/App.tsx` と `src/App.css` を本リポジトリからコピー。

### 3. バックエンド

```bash
cd ../backend
```

`requirements.txt` を作成：

```
bedrock-agentcore
strands-agents
strands-agents-tools[rss]
```

`src/main.py` を本リポジトリからコピー。

### 4. 起動

ターミナルを2つ開いて、それぞれで以下を実行：

```bash
# バックエンド
cd backend && pip install -r requirements.txt && agentcore dev

# フロントエンド
cd frontend && npm run dev
```


---

## おまけ： AWS CloudShell上で起動する手順

ターミナル1（バックエンド）

```
sudo dnf -y install python3.13 python3.13-pip zip
alias python=python3.13
alias pip=pip3.13

mkdir react-agentcore-local && cd react-agentcore-local
mkdir -p backend/src

npm create vite@latest frontend -- --template react-swc-ts

cd frontend
npm install react-markdown
nano vite.config.ts

cd src
rm App.*
nano App.tsx
nano App.css

cd ../../backend
nano requirements.txt

nano src/main.py

pip install --user -r requirements.txt uv bedrock-agentcore-starter-toolkit
agentcore dev
```

ターミナル2（フロントエンド）

```
cd react-agentcore-local/frontend
npm run dev
```

ターミナル3（Webアクセス）

```
ssh -p 443 -R0:localhost:5173 a.pinggy.io
```