# SkillSphere

![SkillSphere Logo](https://via.placeholder.com/150?text=SkillSphere) <!-- Replace with actual logo URL if available -->

## Overview

SkillSphere is a peer-to-peer (P2P) skill exchange platform that connects users to trade skills in real-time. Whether you're teaching Python programming or learning guitar, SkillSphere facilitates 1:1 exchanges through profiles, matching algorithms, and interactive sessions. Built as a lightweight, scalable web app, it emphasizes simplicity for solo developers while supporting growth into a monetized service.

The platform uses skill matching algorithms (e.g., cosine similarity on skill vectors or embedding-based semantic matching via AI) to recommend partners. It's designed for the gig economy, where users can discover, exchange, and even certify skills in a freemium model.

### Key Features
- **User Profiles**: Create detailed profiles with bio, offered/wanted skills (e.g., programming, languages, arts, cooking, fitness), proficiency levels (1-10 scale), location, and availability.
- **Skill Matching**: Search and get recommendations using algorithms like Euclidean distance, cosine similarity, or AI embeddings for semantic matches (e.g., "ML" matches "machine learning").
- **Discovery**: Users find new skills via a search bar (keyword or category-based), personalized recommendations, trending skills feed, or browsing user listings. Categories include Tech, Languages, Creative Arts, Professional Development, Hobbies, and more.
- **Sessions and Chat**: Schedule 1:1 exchanges with real-time chat via WebSockets. Basic exchanges are free; premium users get priority scheduling and unlimited chats.
- **Monetization Features**: Freemium—free basic access; premium subscriptions for advanced matching, certifications (e.g., badges for completed exchanges), and ad-free experience. Users pay for access to high-demand 1:1 chats with verified experts.
- **Admin Tools**: Moderation for profiles, dispute resolution, and analytics.

### Target Audience
- Learners seeking affordable, personalized skill-building (e.g., students, career changers).
- Experts monetizing niche skills in the gig economy.
- Focus on global users, with potential localization (e.g., for Brazil's growing edtech market).

## Technology Stack

SkillSphere is built with a modern, efficient stack optimized for server-side rendering and minimal JavaScript.

- **Backend**: Go with Gin framework for routing and API handling. Authentication via JWT/OAuth (e.g., integrate with Google/Auth0). Database: PostgreSQL with GORM ORM for ORM interactions; add TimescaleDB for time-series data (e.g., session logs) and pgvector for vector storage in AI-based matching (e.g., skill embeddings). (Alternative: Start with SQLite via Turso for MVP simplicity if not needing vectors immediately.)
- **Frontend**: Templ for server-rendered HTML templates, HTMX for asynchronous updates (e.g., dynamic search results without page reloads), and Alpine.js for lightweight client-side interactivity (e.g., modals, toggles). Use all three for a complete, reactive UI without heavy frameworks—HTMX handles async, Alpine adds sprinkles of JS where needed.
- **Real-Time Features**: Gorilla WebSocket for chat and session notifications.
- **AI Integration**: Optional but recommended for advanced matching—use Google Gemini SDK (Go client) to generate skill embeddings for semantic similarity. This enhances discovery by handling synonyms and related skills.
- **Deployment/Cloud**: Fly.io for easy, global deployment (scales well with Go's efficiency, low-cost tiers). Alternatives: Hetzner for budget VPS (if self-managed) or Google Cloud Platform (GCP) for seamless Gemini integration and managed Postgres.
- **Other Tools**: Stripe for payments, Prometheus for metrics, and Docker for containerization.

### Why This Stack?
- **Pros**: Fast development (2-4 weeks MVP), low overhead, scalable. Go handles concurrency for real-time features; server-side focus reduces JS complexity.
- **Cons**: For complex UIs (e.g., drag-and-drop), add more JS sparingly. Start as a Progressive Web App (PWA) for mobile; native apps later.
- **AI Decision**: Yes, for better matching—Gemini provides cost-effective embeddings without heavy ML training. Skip for MVP if focusing on basic algorithms.

## Installation and Setup

### Prerequisites
- Go 1.21+
- PostgreSQL 15+ (with pgvector extension installed via `CREATE EXTENSION vector;`)

### Steps
1. Clone the repo:
   ```
   git clone https://github.com/yourusername/skillsphere.git
   cd skillsphere
   ```

2. Install dependencies:
   ```
   go mod tidy
   # For frontend: If using any JS, npm install (e.g., for Alpine)
   ```

3. Set up environment variables (`.env` file):
   ```
   DATABASE_URL=postgres://user:pass@localhost:5432/skillsphere
   JWT_SECRET=your-secret-key
   GEMINI_API_KEY=your-gemini-key (if using AI)
   STRIPE_KEY=sk_test_...
   ```

4. Initialize database:
   ```
   go run cmd/migrate/main.go  # Assuming a migration script with GORM
   ```

5. Run the server:
   ```
   air  # For hot-reloading (install via go install github.com/air-verse/air@latest)
   # Or: go run cmd/server/main.go
   ```
   Access at `http://localhost:8080`.

6. Deploy to Fly.io:
    - Install Fly CLI: `curl -L https://fly.io/install.sh | sh`
    - `fly launch` (follow prompts for Postgres add-on).
    - Add secrets: `fly secrets set DATABASE_URL=...`

## Usage

- **Sign Up/Login**: Via email/OAuth.
- **Create Profile**: Add skills (e.g., "Go Programming - Level 8" offered, "Spanish - Level 3" wanted).
- **Search & Match**: Enter skills; get sorted recommendations via algorithms.
- **Exchange**: Schedule sessions; chat in real-time.
- **Premium**: Subscribe via Stripe for perks like paying for exclusive 1:1 access.

For code examples of matching algorithms, see `internal/matching/` directory (e.g., cosine similarity in Go using gonum).

## Roadmap
- MVP: Profiles, basic matching, chat (Weeks 1-4).
- V1: AI integration, payments.
- Future: Mobile app, certifications via blockchain badges.

For questions, open an issue or contact [your.email@example.com].

## License

MIT License. See [LICENSE](LICENSE) for details.
---

## SkillSphere Business Plan

### Executive Summary
SkillSphere is a P2P skill exchange platform launching as an MVP in 2-4 weeks, targeting the intersection of the gig economy and online learning markets. With a freemium model, it connects users for skill trades (e.g., tech, languages) using advanced matching algorithms. Projected revenue from subscriptions, premium chats, and ads/partnerships.

Market opportunity: The global gig economy is valued at ~$582B in 2025, online learning at ~$353B, and sharing economy at ~$246B. SkillSphere differentiates with real-time P2P focus, AI-driven discovery, and low-barrier entry.

Goal: Achieve 10K users in Year 1, $500K revenue by Year 2. Solo-founder viable, with scalable tech.

### Market Analysis
- **Size & Growth**: Gig economy: $582.2B by 2025, with 70M+ US workers (36% of workforce). Online learning: $320-353B in 2025, growing 12-14% CAGR to $842B by 2030. P2P/sharing subsets: $194B in 2024 to $246B in 2025.
- **Trends**: Rise in remote work, skill gaps (e.g., AI/tech demand). In emerging markets like Brazil, edtech booms (~$3B by 2025 from prior data). Users seek personalized, affordable alternatives to courses.
- **Target Segments**: 18-45 year-olds in tech, education, creative fields. Initial focus: Global, with SEO for niches like "P2P coding exchange."
- **Competitive Analysis**: Direct competitors include Preply (language tutoring), Skillshare (class-based, not pure P2P), and alternatives like Udemy, Coursera, LinkedIn Learning, MasterClass. P2P platforms: Teachable/Kajabi for creators, but less exchange-focused. Differentiation: Free basic P2P, AI matching, real-time chat. Weaknesses: Established players have scale; SkillSphere starts lean.

### Product Description
- **Core Offering**: P2P exchanges of any skills (e.g., programming, foreign languages, music, cooking, business). Profiles include bio, skill lists with proficiencies, ratings.
- **User Flow**: Sign up → Build profile → Search/discover (algorithms recommend based on wanted/offered skills) → Match & chat (pay for premium 1:1 with high-demand users) → Exchange session → Rate/certify.
- **MVP Scope**: Profiles, basic matching (cosine similarity), chat, search. Expand to AI (Gemini for embeddings), categories.
- **Tech & Operations**: As in README. Development: Solo, 2-4 weeks MVP. Hosting: Fly.io (~$5-20/month initially). Support: Community forums, email.

### Monetization Strategy
- **Freemium Model**: Basic free (limited matches/chats). Premium: $5-20/month for unlimited chats, priority matching, certifications.
- **Additional Revenue**:
    - Pay-per-chat: Users pay $1-10 for 1:1 access to premium profiles (e.g., verified experts).
    - Ads/Partnerships: Targeted ads from edtech firms; affiliate links (e.g., tools for skills).
    - Certifications: $10-50 for badges post-exchange.
- **Projections**: Year 1: 10K users, 10% premium conversion → $60K revenue. Scale to $500K by Year 2 via marketing.

### Marketing & Growth Strategy
- **Acquisition**: SEO (e.g., "free skill exchange app"), social media (Reddit, LinkedIn, X), content marketing (blogs on skill-building). Partnerships with influencers in niches.
- **Retention**: Email newsletters with recommendations, gamification (badges, streaks).
- **Metrics**: Track sign-ups, retention, match success rate. Budget: $1K/month initial (ads/SEO tools).
- **Launch Plan**: Beta via landing page (built with Templ), invite-only, then public.

### Operations & Team
- **Founder-Led**: Solo initially; outsource design if needed.
- **Risks**: Data privacy (GDPR compliance), moderation (AI flags). Mitigation: JWT auth, user reports.
- **Legal**: Incorporate as LLC; terms for exchanges (no liability).

### Financial Projections
- **Startup Costs**: $1-5K (domain, hosting, API keys).
- **Revenue Model**: Subscriptions (70%), pay-per-chat (20%), ads (10%).
- **Break-Even**: Month 6 at 1K premium users.
- **3-Year Forecast**:
    - Year 1: Revenue $100K, Expenses $50K (Profit $50K).
    - Year 2: $500K revenue.
    - Year 3: $2M (with 100K users).
      Assumptions: 20% MoM growth, low churn.

This plan is adaptable—validate MVP feedback for pivots. For refinements, consult advisors.

### Overview of Skill Matching Algorithms

Skill matching algorithms are computational methods used to pair individuals, jobs, or resources based on skills, proficiencies, or requirements. In the context of a P2P skill exchange platform like SkillSphere, these algorithms compare what one user offers (e.g., teaching Python) against what another needs (e.g., learning Python), often scoring matches for relevance. They range from simple rule-based systems to advanced AI-driven ones, balancing factors like accuracy, scalability, and computational cost. Common applications include HR staffing, recommendation systems, and gig platforms.

Key considerations in design:
- **Input Representation**: Skills as vectors (e.g., proficiency levels from 1-10) or embeddings (semantic vectors from NLP models).
- **Scoring**: Measure similarity or distance between profiles.
- **Additional Factors**: Incorporate location, availability, or user ratings for refined matches.
- **Challenges**: Handling synonyms (e.g., "ML" vs. "machine learning"), sparse data (missing skills), and scalability for large user bases.

Below, I'll detail common algorithms, with examples of how they work and potential implementations in Go (using libraries like gonum for vector math and GORM for database integration where relevant).

### 1. Distance-Based Algorithms (e.g., Euclidean/Cartesian Distance)
These treat skills as points in multi-dimensional space, where each skill is a dimension and proficiency is a coordinate. The "closeness" of two profiles is the geometric distance—smaller distances indicate better matches.

- **How It Works**:
    - Represent user profiles as vectors: e.g., User A: [Java: 8, SQL: 5, Python: 0] → Vector [8, 5, 0].
    - Compute distance to a query vector (e.g., [7, 6, 3]).
    - Formula: Euclidean Distance = √Σ (user_i - query_i)². For efficiency, use squared distance (skip the square root).
    - Missing skills default to 0.
    - Sort results by ascending distance for top matches.

- **Pros**: Simple, fast for databases; no training data needed.
- **Cons**: Doesn't handle semantic similarities (e.g., "Java" and "C#" as related).
- **Implementation Example**: In Go, compute distances in-memory after fetching profiles (e.g., via GORM). Use gonum/floats for vector operations.
  ```go
  package main

  import (
  	"fmt"
  	"math"
  	"gonum.org/v1/gonum/floats"
  )

  // UserProfile represents a skill vector (e.g., [Java, SQL, Python] proficiencies)
  type UserProfile struct {
  	ID     int
  	Vector []float64
  }

  func euclideanDistance(a, b []float64) float64 {
  	if len(a) != len(b) {
  		panic("Vectors must be same length")
  	}
  	diff := make([]float64, len(a))
  	floats.SubTo(diff, a, b) // diff = a - b
  	floats.Mul(diff, diff)   // square each element
  	return math.Sqrt(floats.Sum(diff))
  }

  func main() {
  	query := []float64{7, 6, 3}
  	users := []UserProfile{
  		{ID: 1, Vector: []float64{8, 5, 0}},
  		{ID: 2, Vector: []float64{6, 7, 4}},
  	}

  	for _, u := range users {
  		dist := euclideanDistance(u.Vector, query)
  		fmt.Printf("User %d distance: %.2f\n", u.ID, dist)
  	}
  	// Output: Sort users by dist for matches
  }
  ```
- **Use in SkillSphere**: Ideal for MVP with fixed skill lists; extend with weights for critical skills. Integrate into a Gin handler: Fetch users from DB, compute distances, return sorted JSON.

### 2. Similarity-Based Algorithms (e.g., Cosine Similarity)
These measure the angle between vectors, focusing on direction rather than magnitude—useful for sparse or varying-length profiles.

- **How It Works**:
    - Vectors as above, but normalize for length.
    - Formula: Cosine Similarity = (A · B) / (||A|| ||B||), where · is dot product (Σ A_i * B_i), and || || is magnitude (√Σ X_i²).
    - Scores range from -1 (opposite) to 1 (identical); threshold e.g., >0.7 for matches.
    - Handles zeros well (e.g., irrelevant skills don't penalize).

- **Pros**: Robust to scale differences; integrates with search engines like Elasticsearch.
- **Cons**: Ignores absolute proficiency levels if not weighted.
- **Implementation Example**: In Go with gonum/floats for dot product and norms.
  ```go
  package main

  import (
  	"fmt"
  	"gonum.org/v1/gonum/floats"
  )

  func cosineSimilarity(a, b []float64) float64 {
  	if len(a) != len(b) {
  		panic("Vectors must be same length")
  	}
  	dot := floats.Dot(a, b)
  	normA := floats.Norm(a, 2) // L2 norm
  	normB := floats.Norm(b, 2)
  	if normA == 0 || normB == 0 {
  		return 0
  	}
  	return dot / (normA * normB)
  }

  func main() {
  	query := []float64{7, 6, 3}
  	users := [][]float64{
  		{8, 5, 0},
  		{6, 7, 4},
  	}

  	for i, u := range users {
  		sim := cosineSimilarity(u, query)
  		fmt.Printf("User %d similarity: %.2f\n", i+1, sim)
  	}
  	// Threshold >0.7 for good matches
  }
  ```
- **Use in SkillSphere**: For fuzzy matching; combine with full-text search for skill keywords. In Gin, add to a /match endpoint.

### 3. Machine Learning/Embedding-Based Algorithms (e.g., Word2Vec)
These use NLP to create dense vector representations (embeddings) of skills, capturing semantic relationships (e.g., "Python" close to "programming").

- **How It Works** (Based on Word2Vec Approach):
    - **Step 1: Data Prep**: Collect domain-specific text (e.g., skill descriptions from Wikipedia or job sites). Clean: lowercase, remove punctuation/stopwords.
    - **Step 2: Phrase Detection**: Merge common n-grams (e.g., "machine_learning").
    - **Step 3: Train Model**: Word2Vec generates 300D vectors per skill/word.
    - **Step 4: Matching**: Extract skills from user profiles. Compute cosine similarity between query embeddings and profile embeddings. Threshold (e.g., >0.62) marks a match.
    - **Handling Exact vs. Fuzzy**: First check exact matches, then vector similarity for synonyms.

- **Pros**: Semantic understanding; handles variations like abbreviations.
- **Cons**: Requires training data; compute-intensive for real-time.
- **Implementation Example**: Go doesn't have a native Word2Vec, but you can use pre-trained embeddings (e.g., load from file) and gonum for similarity. For training, integrate with an external service or use a Go port like github.com/danieldk/golinear (simplified). Here's a basic loader and matcher:
  ```go
  package main

  import (
  	"bufio"
  	"fmt"
  	"os"
  	"strconv"
  	"strings"
  	"gonum.org/v1/gonum/floats"
  )

  // LoadEmbeddings from a file (e.g., GloVe format: word vec1 vec2...)
  func LoadEmbeddings(filePath string) (map[string][]float64, error) {
  	embeddings := make(map[string][]float64)
  	file, err := os.Open(filePath)
  	if err != nil {
  		return nil, err
  	}
  	defer file.Close()

  	scanner := bufio.NewScanner(file)
  	for scanner.Scan() {
  		parts := strings.Split(scanner.Text(), " ")
  		word := parts[0]
  		vec := make([]float64, len(parts)-1)
  		for i, v := range parts[1:] {
  			vec[i], _ = strconv.ParseFloat(v, 64) // Ignore errors for simplicity
  		}
  		embeddings[word] = vec
  	}
  	return embeddings, scanner.Err()
  }

  func embeddingSimilarity(a, b []float64) float64 {
  	return floats.Dot(a, b) / (floats.Norm(a, 2) * floats.Norm(b, 2))
  }

  func main() {
  	embeddings, _ := LoadEmbeddings("embeddings.txt") // Assume pre-trained file
  	queryEmb := embeddings["python"]                 // e.g., query skill
  	profileEmb := embeddings["programming"]

  	sim := embeddingSimilarity(queryEmb, profileEmb)
  	fmt.Printf("Similarity: %.2f\n", sim)
  	// For profiles: Average embeddings of multiple skills, then compare
  }
  ```
  For full Word2Vec training in Go, consider libraries like github.com/ynqa/word-embeddings or call an external API (e.g., via HTTP in Gin).
- **Use in SkillSphere**: Integrate with an API like Grok for embeddings; great for natural language skill inputs. Pre-load embeddings in init() for fast queries.

### 4. Other Advanced Algorithms
- **Ontology-Based**: Use knowledge graphs (e.g., skill hierarchies like "programming > Python") for semantic matching. Metric: Graph distance or custom similarity (e.g., based on shared ancestors). Pros: Contextual; Cons: Needs ontology database. In Go, use github.com/heimweh/go-pg-grapher for graph ops.
- **Clustering (e.g., k-Means)**: Group users into skill clusters, then match queries to nearest clusters. Useful for discovery. Implement with gonum/cluster.
- **Rule-Based/Hybrid**: Simple thresholds (e.g., match if >70% skills overlap) combined with ML for ties. Often used in HR tools for quick filtering.
- **AI-Enhanced (e.g., with Ontologies)**: Modern systems use LLMs to build dynamic skill databases, improving accuracy over time.

### Recommendations for SkillSphere
Start with Cosine Similarity on vectorized profiles—it's balanced for your Go backend (use gonum). Add embedding-based for semantics as you scale. Test with synthetic data: e.g., 100 users, measure precision/recall. For real-time, use WebSockets to push matches. Install gonum via `go get gonum.org/v1/gonum`. If you need full Gin integration examples or refinements, provide more specifics!# skillsphere
