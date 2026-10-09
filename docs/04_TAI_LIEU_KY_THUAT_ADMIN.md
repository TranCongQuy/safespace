PHÊ DUYỆT

Vai trò Họ tên Ngày
Tác giả Trần Công Quý 09/10/2026
text

---

## 📄 FILE 4: `docs/04_TAI_LIEU_KY_THUAT_ADMIN.md`

````markdown
# TÀI LIỆU KỸ THUẬT AI - ADMIN / BACKEND

# DỰ ÁN: SAFESPACE

**Mã dự án:** SP-2026-001  
**Phiên bản:** 1.0  
**Ngày:** 09/10/2026  
**Công nghệ:** Supabase (PostgreSQL, Auth, Storage, Realtime)

---

## MỤC LỤC

1. Tổng quan Backend
2. Supabase Setup
3. Database Schema chi tiết
4. Row Level Security (RLS)
5. Authentication
6. Storage
7. Realtime
8. Edge Functions
9. Admin Features
10. Security & Compliance
11. Backup & Recovery
12. Monitoring
13. Deploy & CI/CD

---

## 1. TỔNG QUAN BACKEND

### 1.1 Tại sao dùng Supabase?

| Tiêu chí     | Tự code (Node + MySQL) | Supabase          |
| ------------ | ---------------------- | ----------------- |
| **Setup**    | 2-3 ngày               | 30 phút           |
| **Auth**     | Tự code JWT + bcrypt   | Có sẵn            |
| **Realtime** | Socket.io              | Có sẵn            |
| **Storage**  | Multer + S3            | Có sẵn            |
| **Scaling**  | Tự lo                  | Auto              |
| **Security** | Tự lo                  | RLS built-in      |
| **Cost**     | VPS $10-50/tháng       | Free tier đủ dùng |
| **Phù hợp**  | Khi cần full control   | Hầu hết dự án     |

### 1.2 Kiến trúc tổng quan

┌─────────────────────────────────────────────────┐
│ FRONTEND (React) │
└────────────────────┬────────────────────────────┘
│ HTTPS
▼
┌─────────────────────────────────────────────────┐
│ SUPABASE CLOUD │
│ │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ │ Auth │ │ Database │ │ Storage │ │
│ │ (GoTrue)│ │(Postgres)│ │ (S3) │ │
│ └──────────┘ └──────────┘ └──────────┘ │
│ │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ │ Realtime │ │ Edge │ │ REST API │ │
│ │ (Phoenix)│ │Functions │ │ (PostgREST)│ │
│ └──────────┘ └──────────┘ └──────────┘ │
│ │
│ ┌──────────────────────────────────────┐ │
│ │ Row Level Security (RLS) │ │
│ └──────────────────────────────────────┘ │
└─────────────────────────────────────────────────┘

text

---

## 2. SUPABASE SETUP

### 2.1 Tạo project

1. Vào https://supabase.com
2. Đăng nhập bằng GitHub
3. New Project:
   - **Name**: `safespace`
   - **Database Password**: (lưu lại!)
   - **Region**: `Southeast Asia (Singapore)`
   - **Pricing Plan**: Free

### 2.2 Lấy API Keys

Vào **Settings → API**:

- `Project URL`: `https://xxx.supabase.co`
- `anon public key`: `eyJhbG...`
- `service_role key`: `eyJhbG...` ⚠️ **BÍ MẬT**

### 2.3 Cấu hình Auth

Vào **Authentication → Providers**:

- ✅ Enable Anonymous Sign-ins
- ✅ Enable Email (nếu cần)
- ❌ Disable phone (để ẩn danh)

Vào **Authentication → URL Configuration**:

- **Site URL**: `https://safespace.vercel.app`
- **Redirect URLs**:
  - `http://localhost:5173/**`
  - `https://safespace.vercel.app/**`

---

## 3. DATABASE SCHEMA CHI TIẾT

### 3.1 Sơ đồ ERD

┌──────────────┐
│ auth.users │ (Supabase managed)
└───────┬──────┘
│ 1-1
▼
┌──────────────┐ ┌──────────────┐
│ profiles │ │ supporters │
└───────┬──────┘ └──────────────┘
│
│ 1-n
▼
┌──────────────┐ ┌──────────────┐
│ mood_logs │ │ posts │
└──────────────┘ └───────┬──────┘
│ 1-n
▼
┌──────────────┐
│ comments │
└──────────────┘

┌──────────────┐ ┌──────────────┐
│chat_sessions │ 1-n │chat_messages │
└──────────────┘───────└──────────────┘

┌──────────────┐ ┌──────────────┐
│ resources │ │ hotlines │
└──────────────┘ └──────────────┘

┌──────────────┐ ┌──────────────┐
│ reports │ │ crisis_logs │
└──────────────┘ └──────────────┘

text

### 3.2 Full SQL Schema

```sql
-- ============================================
-- MIGRATION 1: PROFILES
-- ============================================
CREATE TABLE profiles (
  id UUID REFERENCES auth.users(id) ON DELETE CASCADE PRIMARY KEY,
  nickname TEXT UNIQUE,
  avatar_seed TEXT DEFAULT gen_random_uuid()::text,
  role TEXT DEFAULT 'user' CHECK (role IN ('user', 'supporter', 'moderator', 'admin')),
  is_banned BOOLEAN DEFAULT FALSE,
  banned_until TIMESTAMPTZ,
  ban_reason TEXT,
  last_seen_at TIMESTAMPTZ DEFAULT NOW(),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Trigger tạo profile khi có user mới
CREATE OR REPLACE FUNCTION handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO public.profiles (id, nickname)
  VALUES (
    NEW.id,
    'Người dùng ' || SUBSTRING(NEW.id::text, 1, 8)
  );
  RETURN NEW;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE FUNCTION handle_new_user();

-- ============================================
-- MIGRATION 2: MOOD LOGS
-- ============================================
CREATE TABLE mood_logs (
  id BIGSERIAL PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  mood INT NOT NULL CHECK (mood BETWEEN 1 AND 5),
  emotions TEXT[] DEFAULT '{}',
  note TEXT CHECK (char_length(note) <= 500),
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_mood_user_created
  ON mood_logs(user_id, created_at DESC);

-- ============================================
-- MIGRATION 3: POSTS
-- ============================================
CREATE TABLE posts (
  id BIGSERIAL PRIMARY KEY,
  author_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  title TEXT CHECK (char_length(title) <= 200),
  content TEXT NOT NULL CHECK (char_length(content) BETWEEN 1 AND 5000),
  category TEXT NOT NULL CHECK (category IN ('share', 'question', 'support', 'story')),
  is_anonymous BOOLEAN DEFAULT TRUE,
  like_count INT DEFAULT 0,
  comment_count INT DEFAULT 0,
  is_flagged BOOLEAN DEFAULT FALSE,
  is_hidden BOOLEAN DEFAULT FALSE,
  hidden_reason TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_posts_created ON posts(created_at DESC);
CREATE INDEX idx_posts_category ON posts(category);
CREATE INDEX idx_posts_author ON posts(author_id);

-- ============================================
-- MIGRATION 4: COMMENTS
-- ============================================
CREATE TABLE comments (
  id BIGSERIAL PRIMARY KEY,
  post_id BIGINT REFERENCES posts(id) ON DELETE CASCADE NOT NULL,
  author_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  content TEXT NOT NULL CHECK (char_length(content) BETWEEN 1 AND 1000),
  is_anonymous BOOLEAN DEFAULT TRUE,
  is_hidden BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_comments_post ON comments(post_id, created_at);

-- ============================================
-- MIGRATION 5: REACTIONS
-- ============================================
CREATE TABLE reactions (
  id BIGSERIAL PRIMARY KEY,
  post_id BIGINT REFERENCES posts(id) ON DELETE CASCADE,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  type TEXT NOT NULL CHECK (type IN ('like', 'hug', 'support')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE (post_id, user_id, type)
);

-- ============================================
-- MIGRATION 6: SUPPORTERS
-- ============================================
CREATE TABLE supporters (
  id UUID REFERENCES auth.users(id) ON DELETE CASCADE PRIMARY KEY,
  display_name TEXT NOT NULL,
  bio TEXT CHECK (char_length(bio) <= 500),
  specialties TEXT[] DEFAULT '{}',
  is_online BOOLEAN DEFAULT FALSE,
  is_verified BOOLEAN DEFAULT FALSE,
  total_sessions INT DEFAULT 0,
  avg_rating DECIMAL(3,2) DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- MIGRATION 7: CHAT
-- ============================================
CREATE TABLE chat_sessions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  supporter_id UUID REFERENCES auth.users(id) ON DELETE SET NULL,
  status TEXT DEFAULT 'active' CHECK (status IN ('active', 'ended', 'escalated')),
  started_at TIMESTAMPTZ DEFAULT NOW(),
  ended_at TIMESTAMPTZ,
  user_rating INT CHECK (user_rating BETWEEN 1 AND 5),
  user_feedback TEXT
);

CREATE INDEX idx_sessions_status ON chat_sessions(status, started_at DESC);

CREATE TABLE chat_messages (
  id BIGSERIAL PRIMARY KEY,
  session_id UUID REFERENCES chat_sessions(id) ON DELETE CASCADE,
  sender_id UUID REFERENCES auth.users(id) ON DELETE SET NULL,
  content TEXT NOT NULL CHECK (char_length(content) <= 2000),
  is_system BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_messages_session ON chat_messages(session_id, created_at);

-- Auto-delete messages sau 30 ngày
CREATE OR REPLACE FUNCTION cleanup_old_messages()
RETURNS void AS $$
BEGIN
  DELETE FROM chat_messages
  WHERE created_at < NOW() - INTERVAL '30 days';
END;
$$ LANGUAGE plpgsql;

-- ============================================
-- MIGRATION 8: CRISIS LOGS
-- ============================================
CREATE TABLE crisis_logs (
  id BIGSERIAL PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) ON DELETE SET NULL,
  trigger_type TEXT NOT NULL CHECK (trigger_type IN ('sos_button', 'keyword_detected', 'manual')),
  crisis_level TEXT CHECK (crisis_level IN ('low', 'medium', 'high', 'critical')),
  context TEXT,
  action_taken TEXT,
  resolved BOOLEAN DEFAULT FALSE,
  resolved_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- MIGRATION 9: RESOURCES
-- ============================================
CREATE TABLE resources (
  id BIGSERIAL PRIMARY KEY,
  title TEXT NOT NULL,
  content TEXT NOT NULL,
  excerpt TEXT,
  category TEXT CHECK (category IN ('article', 'exercise', 'story', 'guide')),
  tags TEXT[] DEFAULT '{}',
  thumbnail_url TEXT,
  author_id UUID REFERENCES auth.users(id),
  is_published BOOLEAN DEFAULT TRUE,
  view_count INT DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- MIGRATION 10: HOTLINES
-- ============================================
CREATE TABLE hotlines (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  phone TEXT NOT NULL,
  description TEXT,
  available_hours TEXT DEFAULT '24/7',
  region TEXT DEFAULT 'Toàn quốc',
  priority INT DEFAULT 0,
  is_active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Seed data
INSERT INTO hotlines (name, phone, description, available_hours, priority) VALUES
('Tổng đài Quốc gia Bảo vệ Trẻ em', '111', 'Hỗ trợ trẻ em bị bạo lực, xâm hại', '24/7', 1),
('Đường dây nóng Sức khỏe Tâm thần', '1800 599 920', 'Tư vấn tâm lý miễn phí', '24/7', 2),
('BV Tâm thần Trung ương 1', '024 3825 2240', 'Khám và điều trị tâm thần', 'Giờ hành chính', 3),
('BV Tâm thần TP.HCM', '028 3822 3470', 'Khám và điều trị tâm thần', 'Giờ hành chính', 4);

-- ============================================
-- MIGRATION 11: REPORTS
-- ============================================
CREATE TABLE reports (
  id BIGSERIAL PRIMARY KEY,
  reporter_id UUID REFERENCES auth.users(id) ON DELETE SET NULL,
  target_type TEXT NOT NULL CHECK (target_type IN ('post', 'comment', 'user', 'chat')),
  target_id BIGINT NOT NULL,
  reason TEXT NOT NULL CHECK (reason IN ('spam', 'harassment', 'harmful', 'personal_info', 'other')),
  description TEXT CHECK (char_length(description) <= 500),
  status TEXT DEFAULT 'pending' CHECK (status IN ('pending', 'resolved', 'dismissed')),
  resolved_by UUID REFERENCES auth.users(id),
  resolved_at TIMESTAMPTZ,
  resolution_note TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_reports_status ON reports(status, created_at DESC);

-- ============================================
-- MIGRATION 12: AUDIT LOG
-- ============================================
CREATE TABLE audit_log (
  id BIGSERIAL PRIMARY KEY,
  actor_id UUID REFERENCES auth.users(id),
  action TEXT NOT NULL,
  target_type TEXT,
  target_id BIGINT,
  metadata JSONB,
  ip_address INET,
  user_agent TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_audit_actor ON audit_log(actor_id, created_at DESC);
CREATE INDEX idx_audit_action ON audit_log(action, created_at DESC);

-- ============================================
-- TRIGGERS
-- ============================================

-- Auto-update updated_at
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_profiles_updated
  BEFORE UPDATE ON profiles
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER trg_posts_updated
  BEFORE UPDATE ON posts
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

-- Auto-increment comment_count
CREATE OR REPLACE FUNCTION update_comment_count()
RETURNS TRIGGER AS $$
BEGIN
  IF TG_OP = 'INSERT' THEN
    UPDATE posts SET comment_count = comment_count + 1 WHERE id = NEW.post_id;
  ELSIF TG_OP = 'DELETE' THEN
    UPDATE posts SET comment_count = comment_count - 1 WHERE id = OLD.post_id;
  END IF;
  RETURN NULL;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_comment_count
  AFTER INSERT OR DELETE ON comments
  FOR EACH ROW EXECUTE FUNCTION update_comment_count();
4. ROW LEVEL SECURITY (RLS)
4.1 Nguyên tắc
Deny by default — mọi thứ bị chặn, chỉ mở khi cần

User chỉ xem được data của mình

Admin có quyền cao hơn

4.2 Enable RLS
sql
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE mood_logs ENABLE ROW LEVEL SECURITY;
ALTER TABLE posts ENABLE ROW LEVEL SECURITY;
ALTER TABLE comments ENABLE ROW LEVEL SECURITY;
ALTER TABLE chat_sessions ENABLE ROW LEVEL SECURITY;
ALTER TABLE chat_messages ENABLE ROW LEVEL SECURITY;
ALTER TABLE crisis_logs ENABLE ROW LEVEL SECURITY;
ALTER TABLE reports ENABLE ROW LEVEL SECURITY;
4.3 Policies chi tiết
sql
-- ============================================
-- PROFILES
-- ============================================

-- Ai cũng xem được profile public (chỉ nickname, avatar)
CREATE POLICY "Public profiles are viewable"
  ON profiles FOR SELECT
  USING (TRUE);

-- User cập nhật profile của mình
CREATE POLICY "Users update own profile"
  ON profiles FOR UPDATE
  TO authenticated
  USING (auth.uid() = id)
  WITH CHECK (
    auth.uid() = id
    AND role = (SELECT role FROM profiles WHERE id = auth.uid())  -- không tự đổi role
  );

-- Admin update tất cả
CREATE POLICY "Admins manage all profiles"
  ON profiles FOR ALL
  TO authenticated
  USING (
    EXISTS (
      SELECT 1 FROM profiles
      WHERE id = auth.uid() AND role = 'admin'
    )
  );

-- ============================================
-- MOOD LOGS
-- ============================================

-- User CHỈ xem mood logs của mình
CREATE POLICY "Users view own mood logs"
  ON mood_logs FOR SELECT
  TO authenticated
  USING (auth.uid() = user_id);

CREATE POLICY "Users insert own mood logs"
  ON mood_logs FOR INSERT
  TO authenticated
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users update own mood logs"
  ON mood_logs FOR UPDATE
  TO authenticated
  USING (auth.uid() = user_id);

CREATE POLICY "Users delete own mood logs"
  ON mood_logs FOR DELETE
  TO authenticated
  USING (auth.uid() = user_id);

-- ============================================
-- POSTS
-- ============================================

-- Ai cũng xem posts không bị hide
CREATE POLICY "Anyone views unhidden posts"
  ON posts FOR SELECT
  USING (is_hidden = FALSE);

-- Author xem posts của mình (kể cả hidden)
CREATE POLICY "Authors view own posts"
  ON posts FOR SELECT
  TO authenticated
  USING (auth.uid() = author_id);

-- Auth user tạo post
CREATE POLICY "Authenticated create posts"
  ON posts FOR INSERT
  TO authenticated
  WITH CHECK (auth.uid() = author_id);

-- Author sửa post của mình trong 1 giờ
CREATE POLICY "Authors update own posts"
  ON posts FOR UPDATE
  TO authenticated
  USING (
    auth.uid() = author_id
    AND created_at > NOW() - INTERVAL '1 hour'
  );

-- Moderator/Admin có quyền đầy đủ
CREATE POLICY "Moderators manage posts"
  ON posts FOR ALL
  TO authenticated
  USING (
    EXISTS (
      SELECT 1 FROM profiles
      WHERE id = auth.uid()
      AND role IN ('moderator', 'admin')
    )
  );

-- ============================================
-- COMMENTS
-- ============================================

CREATE POLICY "Anyone views unhidden comments"
  ON comments FOR SELECT
  USING (is_hidden = FALSE);

CREATE POLICY "Authenticated create comments"
  ON comments FOR INSERT
  TO authenticated
  WITH CHECK (auth.uid() = author_id);

CREATE POLICY "Authors update own comments"
  ON comments FOR UPDATE
  TO authenticated
  USING (auth.uid() = author_id);

CREATE POLICY "Authors delete own comments"
  ON comments FOR DELETE
  TO authenticated
  USING (auth.uid() = author_id);

-- ============================================
-- CHAT
-- ============================================

-- User xem session của mình
CREATE POLICY "Users view own chat sessions"
  ON chat_sessions FOR SELECT
  TO authenticated
  USING (auth.uid() = user_id OR auth.uid() = supporter_id);

CREATE POLICY "Users create own sessions"
  ON chat_sessions FOR INSERT
  TO authenticated
  WITH CHECK (auth.uid() = user_id);

-- Messages
CREATE POLICY "Users view own chat messages"
  ON chat_messages FOR SELECT
  TO authenticated
  USING (
    EXISTS (
      SELECT 1 FROM chat_sessions
      WHERE id = session_id
      AND (user_id = auth.uid() OR supporter_id = auth.uid())
    )
  );

CREATE POLICY "Users send messages in own sessions"
  ON chat_messages FOR INSERT
  TO authenticated
  WITH CHECK (
    auth.uid() = sender_id
    AND EXISTS (
      SELECT 1 FROM chat_sessions
      WHERE id = session_id
      AND status = 'active'
      AND (user_id = auth.uid() OR supporter_id = auth.uid())
    )
  );

-- ============================================
-- CRISIS LOGS
-- ============================================

-- User xem log của mình
CREATE POLICY "Users view own crisis logs"
  ON crisis_logs FOR SELECT
  TO authenticated
  USING (auth.uid() = user_id);

CREATE POLICY "Users create crisis logs"
  ON crisis_logs FOR INSERT
  TO authenticated
  WITH CHECK (auth.uid() = user_id);

-- Admin xem tất cả
CREATE POLICY "Admins view all crisis logs"
  ON crisis_logs FOR SELECT
  TO authenticated
  USING (
    EXISTS (
      SELECT 1 FROM profiles
      WHERE id = auth.uid() AND role = 'admin'
    )
  );

-- ============================================
-- REPORTS
-- ============================================

CREATE POLICY "Users create reports"
  ON reports FOR INSERT
  TO authenticated
  WITH CHECK (auth.uid() = reporter_id);

CREATE POLICY "Users view own reports"
  ON reports FOR SELECT
  TO authenticated
  USING (auth.uid() = reporter_id);

CREATE POLICY "Mods manage reports"
  ON reports FOR ALL
  TO authenticated
  USING (
    EXISTS (
      SELECT 1 FROM profiles
      WHERE id = auth.uid()
      AND role IN ('moderator', 'admin')
    )
  );

-- ============================================
-- HOTLINES & RESOURCES (public read)
-- ============================================

ALTER TABLE hotlines ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Anyone reads hotlines"
  ON hotlines FOR SELECT
  USING (is_active = TRUE);

ALTER TABLE resources ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Anyone reads published resources"
  ON resources FOR SELECT
  USING (is_published = TRUE);

CREATE POLICY "Admins manage resources"
  ON resources FOR ALL
  TO authenticated
  USING (
    EXISTS (
      SELECT 1 FROM profiles
      WHERE id = auth.uid()
      AND role IN ('moderator', 'admin')
    )
  );
5. AUTHENTICATION
5.1 Anonymous Sign-in
js
// Frontend
const { data, error } = await supabase.auth.signInAnonymously();
Ưu điểm:

Không cần email/phone

Bảo vệ privacy user

Vẫn có user_id để track

Limitations:

Mất session khi xóa cache

Không restore được account

5.2 Upgrade to Permanent Account (Optional)
js
// User có thể nâng cấp sau
const { data, error } = await supabase.auth.updateUser({
  email: 'user@example.com',
  password: 'secure_password'
});
5.3 Auth Hooks
js
// Custom hook
export function useAuth() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    supabase.auth.getUser().then(({ data }) => {
      setUser(data.user);
    });

    const { data: sub } = supabase.auth.onAuthStateChange(
      (_, session) => setUser(session?.user ?? null)
    );

    return () => sub.subscription.unsubscribe();
  }, []);

  return { user, isAuthenticated: !!user };
}
6. STORAGE
6.1 Buckets
Bucket	Public	Dùng cho
avatars	✅	Avatar users
resources	✅	Ảnh bài viết
evidence	❌	Ảnh bằng chứng (private)
6.2 Tạo bucket
Vào Storage → New bucket:

Name: avatars

Public: ✅

6.3 Storage Policies
sql
-- Public read
CREATE POLICY "Public read avatars"
  ON storage.objects FOR SELECT
  USING (bucket_id = 'avatars');

-- Auth upload
CREATE POLICY "Auth users upload own avatar"
  ON storage.objects FOR INSERT
  TO authenticated
  WITH CHECK (
    bucket_id = 'avatars'
    AND (storage.foldername(name))[1] = auth.uid()::text
  );
6.4 Upload từ Frontend
js
async function uploadAvatar(file) {
  const { data: { user } } = await supabase.auth.getUser();

  const filePath = `${user.id}/avatar-${Date.now()}.jpg`;

  const { error } = await supabase.storage
    .from('avatars')
    .upload(filePath, file, {
      cacheControl: '3600',
      upsert: true
    });

  if (error) throw error;

  const { data } = supabase.storage
    .from('avatars')
    .getPublicUrl(filePath);

  return data.publicUrl;
}
7. REALTIME
7.1 Enable Realtime
Vào Database → Replication → Supabase Realtime:

Enable cho bảng: chat_messages, posts, comments

7.2 Subscribe từ Frontend
js
useEffect(() => {
  const channel = supabase
    .channel(`chat:${sessionId}`)
    .on(
      'postgres_changes',
      {
        event: 'INSERT',
        schema: 'public',
        table: 'chat_messages',
        filter: `session_id=eq.${sessionId}`
      },
      (payload) => {
        setMessages(prev => [...prev, payload.new]);
      }
    )
    .subscribe();

  return () => {
    supabase.removeChannel(channel);
  };
}, [sessionId]);
8. EDGE FUNCTIONS
8.1 Crisis Detection Function
ts
// supabase/functions/detect-crisis/index.ts
import { serve } from 'https://deno.land/std/http/server.ts';
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2';

const CRISIS_KEYWORDS = [
  'tự tử', 'tự sát', 'kết thúc cuộc đời',
  'không muốn sống', 'chết đi'
];

serve(async (req) => {
  const { text } = await req.json();

  const lower = text.toLowerCase();
  const found = CRISIS_KEYWORDS.filter(kw => lower.includes(kw));

  const isCrisis = found.length > 0;

  if (isCrisis) {
    // Log crisis event
    const supabase = createClient(
      Deno.env.get('SUPABASE_URL')!,
      Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')!
    );

    await supabase.from('crisis_logs').insert({
      trigger_type: 'keyword_detected',
      context: found.join(', ')
    });
  }

  return new Response(
    JSON.stringify({ isCrisis, keywords: found }),
    { headers: { 'Content-Type': 'application/json' } }
  );
});
8.2 Cleanup Function (Cron)
ts
// supabase/functions/cleanup-old-data/index.ts
serve(async () => {
  const supabase = createClient(/* ... */);

  // Xóa chat messages cũ hơn 30 ngày
  const cutoff = new Date();
  cutoff.setDate(cutoff.getDate() - 30);

  const { count } = await supabase
    .from('chat_messages')
    .delete({ count: 'exact' })
    .lt('created_at', cutoff.toISOString());

  return new Response(JSON.stringify({ deleted: count }));
});
9. ADMIN FEATURES
9.1 Admin Roles
Role	Quyền
user	Cơ bản
supporter	Chat + Support
moderator	Duyệt content
admin	Toàn quyền
9.2 Admin Dashboard Queries
sql
-- Stats tổng quan
CREATE OR REPLACE FUNCTION get_admin_stats()
RETURNS JSON AS $$
DECLARE
  result JSON;
BEGIN
  SELECT json_build_object(
    'total_users', (SELECT COUNT(*) FROM profiles),
    'total_posts', (SELECT COUNT(*) FROM posts WHERE is_hidden = FALSE),
    'pending_reports', (SELECT COUNT(*) FROM reports WHERE status = 'pending'),
    'active_chats', (SELECT COUNT(*) FROM chat_sessions WHERE status = 'active'),
    'crises_today', (SELECT COUNT(*) FROM crisis_logs
                     WHERE created_at > CURRENT_DATE)
  ) INTO result;
  RETURN result;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;
9.3 Moderator Actions
js
// adminService.js
export const adminService = {
  async hidePost(postId, reason) {
    const { data: { user } } = await supabase.auth.getUser();

    await supabase
      .from('posts')
      .update({
        is_hidden: TRUE,
        hidden_reason: reason
      })
      .eq('id', postId);

    // Audit log
    await supabase.from('audit_log').insert({
      actor_id: user.id,
      action: 'hide_post',
      target_type: 'post',
      target_id: postId,
      metadata: { reason }
    });
  },

  async banUser(userId, days, reason) {
    const bannedUntil = new Date();
    bannedUntil.setDate(bannedUntil.getDate() + days);

    await supabase
      .from('profiles')
      .update({
        is_banned: TRUE,
        banned_until: bannedUntil.toISOString(),
        ban_reason: reason
      })
      .eq('id', userId);
  },

  async resolveReport(reportId, action, note) {
    await supabase
      .from('reports')
      .update({
        status: 'resolved',
        resolution_note: note,
        resolved_at: new Date().toISOString()
      })
      .eq('id', reportId);

    // Apply action based on type
    // ...
  }
};
10. SECURITY & COMPLIANCE
10.1 Checklist bảo mật
☑ RLS enabled cho tất cả bảng
☑ Auth required cho write operations
☑ Rate limiting (Supabase built-in)
☑ HTTPS only
☑ Encryption at rest
☑ Encryption in transit
☑ No PII stored
☑ Anonymous auth
☑ Audit log
☑ Auto-delete old data
10.2 Tuân thủ Nghị định 13/2023
Yêu cầu	Cách đáp ứng
Đồng ý của chủ thể	Terms khi đăng ký
Quyền truy cập	User xem data của mình
Quyền xóa	API xóa account
Thông báo vi phạm	Email notification
Bảo mật dữ liệu	Encryption + RLS
10.3 Data Retention
Loại data	Retention	Lý do
Chat messages	30 ngày	Privacy
Mood logs	User xóa	Quyền user
Posts	Vĩnh viễn (nếu public)	Community value
Crisis logs	1 năm (ẩn danh)	Analytics
Audit log	2 năm	Compliance
11. BACKUP & RECOVERY
11.1 Auto Backup
Supabase tự động backup hàng ngày (Pro plan).

11.2 Manual Backup
bash
# Export schema + data
pg_dump -h db.xxx.supabase.co \
  -U postgres \
  -d postgres \
  -f backup.sql

# Restore
psql -h db.xxx.supabase.co -U postgres -d postgres -f backup.sql
11.3 Recovery Plan
RTO	RPO
4 giờ	24 giờ
12. MONITORING
12.1 Supabase Dashboard
Database → Query Performance — Slow queries

Auth → Users — Signups

Logs → Edge Functions — Errors

Reports → Database — Usage

12.2 Custom Alerts
sql
-- Alert nếu có > 50 crisis trong 1 giờ
CREATE OR REPLACE FUNCTION check_crisis_alert()
RETURNS void AS $$
DECLARE
  crisis_count INT;
BEGIN
  SELECT COUNT(*) INTO crisis_count
  FROM crisis_logs
  WHERE created_at > NOW() - INTERVAL '1 hour';

  IF crisis_count > 50 THEN
    -- Send alert
    INSERT INTO audit_log (action, metadata)
    VALUES ('HIGH_CRISIS_ALERT', jsonb_build_object('count', crisis_count));
  END IF;
END;
$$ LANGUAGE plpgsql;
13. DEPLOY & CI/CD
13.1 Supabase Migrations
bash
# Install CLI
npm i -g supabase

# Init
supabase init

# Link to project
supabase link --project-ref xxx

# Create migration
supabase migration new create_posts

# Apply
supabase db push
13.2 GitHub Actions
yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install deps
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-args: '--prod'
14. KẾT LUẬN
Tài liệu này mô tả toàn bộ backend/Admin của SafeSpace:

✅ Supabase-based (không cần tự code)

✅ RLS bảo mật cao

✅ Anonymous auth

✅ Realtime chat

✅ Storage cho files

✅ Edge Functions

✅ Audit log

✅ Compliance

Điểm mạnh:

Setup nhanh (1 ngày)

Không cần DevOps

Auto scaling

Security tốt

Hạn chế:

Phụ thuộc Supabase

Free tier có giới hạn

Vendor lock-in
```
````
