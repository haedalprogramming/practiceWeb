# 2) 컴포넌트 설계

## 2-1) 컴포넌트 설계 패턴 (Atomic Design)

React와 같은 컴포넌트 기반 프레임워크를 사용하여 웹 애플리케이션을 만들 때, 컴포넌트를 어떻게 구성하고 관리하는지는 매우 중요합니다. 잘 설계된 컴포넌트는 재사용성을 높이고, 코드의 유지보수를 쉽게 만들며, 여러 개발자가 협업할 때 효율성을 높여줍니다.

이번 강의에서는 **Atomic Design (아토믹 디자인)**이라는 컴포넌트 설계 패턴에 대해 알아보겠습니다. Atomic Design은 복잡한 UI(사용자 인터페이스)를 작은 단위로 분해하고, 이 작은 단위들을 조합하여 더 큰 UI를 만드는 방법론입니다.

### 2-1-1) Atomic Design의 5단계

Atomic Design은 화학의 원자 개념에서 영감을 받아 UI를 다음 5가지 단계로 나눕니다.

1.  **원자 (Atoms)**
2.  **분자 (Molecules)**
3.  **유기체 (Organisms)**
4.  **템플릿 (Templates)**
5.  **페이지 (Pages)**

각 단계는 독립적인 역할을 하며, 이전 단계의 요소들을 조합하여 다음 단계의 요소를 만듭니다.

#### 1) 원자 (Atoms)

원자는 UI의 가장 기본적인 구성 요소입니다. 더 이상 분해할 수 없는 최소 단위이며, 단일 목적을 가집니다.

- 예시: 버튼, 입력 필드, 레이블, 아이콘, 텍스트, 이미지 등

원자는 그 자체로는 큰 의미를 가지지 않지만, 다른 원자들과 결합하여 의미 있는 UI를 만듭니다.

#### 2) 분자 (Molecules)

분자는 두 개 이상의 원자들이 결합하여 만들어진 단위입니다. 특정 기능을 수행하는 작은 UI 그룹을 형성합니다.

- 예시: 검색 폼 (입력 필드 + 버튼), 내비게이션 메뉴 (여러 개의 링크 원자), 사용자 프로필 카드 (이미지 + 이름 텍스트) 등

분자는 재사용 가능한 독립적인 컴포넌트가 됩니다.

#### 3) 유기체 (Organisms)

유기체는 분자들과 원자들이 결합하여 만들어진 더 크고 복잡한 UI 단위입니다. 페이지의 특정 섹션을 구성하며, 독립적인 기능을 수행합니다.

- 예시: 헤더 (로고, 내비게이션 메뉴, 검색 폼), 푸터 (회사 정보, 저작권), 상품 목록 (여러 개의 상품 카드 분자) 등

유기체는 페이지의 주요 영역을 구성하며, 재사용 가능하지만 분자보다는 더 구체적인 맥락을 가집니다.

#### 4) 템플릿 (Templates)

템플릿은 유기체, 분자, 원자들이 배치된 페이지의 골격 구조를 나타냅니다. 실제 데이터가 채워지기 전의 와이어프레임(뼈대)과 같습니다.

- 예시: 블로그 게시물 템플릿 (헤더, 게시물 제목, 내용 영역, 댓글 영역, 푸터), 로그인 페이지 템플릿 (로그인 폼 유기체, 로고) 등

템플릿은 콘텐츠의 구조와 레이아웃에 집중하며, 실제 데이터가 아닌 플레이스홀더(임시 데이터)를 사용합니다.

#### 5) 페이지 (Pages)

페이지는 템플릿에 실제 데이터가 채워진 최종 UI입니다. 사용자가 실제로 보게 되는 화면이며, 템플릿의 특정 인스턴스(실제 모습)입니다.

- 예시: 특정 블로그 게시물 페이지, 특정 사용자의 프로필 페이지, 실제 상품 목록 페이지 등

페이지는 템플릿의 유효성을 검증하고, 다양한 콘텐츠가 어떻게 표시되는지 확인할 수 있는 단계입니다.

### 2-1-2) Atomic Design의 장점

*   **재사용성 향상**: 작은 단위부터 컴포넌트를 만들고 조합하므로, 각 컴포넌트의 재사용성이 높아집니다.
*   **일관된 UI**: 모든 컴포넌트가 체계적으로 설계되므로, 애플리케이션 전체의 UI 일관성을 유지하기 쉽습니다.
*   **유지보수 용이**: 컴포넌트가 명확한 계층 구조를 가지므로, 특정 부분을 수정하거나 개선할 때 영향을 받는 범위를 쉽게 파악할 수 있습니다.
*   **협업 효율성**: 디자이너와 개발자 간의 의사소통을 명확하게 하고, 각자의 역할에 집중할 수 있도록 돕습니다.

> [!TIP]
> Atomic Design은 UI를 체계적으로 분해하고 조합하는 방법을 제공하여, 복잡한 React 애플리케이션을 효율적으로 개발하고 관리하는 데 큰 도움을 줍니다.

### 2-1-3) Atomic Design 실제 코드 예시

이제 Atomic Design의 각 단계를 React 컴포넌트 코드로 어떻게 구현할 수 있는지 간단한 예시를 통해 살펴보겠습니다.

#### 1) 원자 (Atoms) 예시

가장 기본적인 UI 요소인 버튼과 텍스트 입력 필드를 원자로 정의할 수 있습니다.

```jsx
// src/components/atoms/Button.jsx
import React from 'react';

function Button({ children, onClick }) {
  return (
    <button
      onClick={onClick}
      style={{
        backgroundColor: '#007bff',
        color: 'white',
        padding: '10px 15px',
        border: 'none',
        borderRadius: '5px',
        cursor: 'pointer',
      }}
    >
      {children}
    </button>
  );
}

export default Button;

// src/components/atoms/Input.jsx
import React from 'react';

function Input({ placeholder, value, onChange }) {
  return (
    <input
      type="text"
      placeholder={placeholder}
      value={value}
      onChange={onChange}
      style={{
        padding: '10px',
        border: '1px solid #ccc',
        borderRadius: '5px',
      }}
    />
  );
}

export default Input;
```

#### 2) 분자 (Molecules) 예시

원자들을 조합하여 특정 기능을 수행하는 작은 UI 그룹인 검색 폼을 분자로 만들 수 있습니다.

```jsx
// src/components/molecules/SearchForm.jsx
import React, { useState } from 'react';
import Button from '../atoms/Button'; // 원자 컴포넌트 임포트
import Input from '../atoms/Input';   // 원자 컴포넌트 임포트

function SearchForm({ onSearch }) {
  const [searchTerm, setSearchTerm] = useState('');

  const handleChange = (e) => {
    setSearchTerm(e.target.value);
  };

  const handleSubmit = () => {
    onSearch(searchTerm);
  };

  return (
    <div style={{ display: 'flex', gap: '10px' }}>
      <Input
        placeholder="검색어를 입력하세요"
        value={searchTerm}
        onChange={handleChange}
      />
      <Button onClick={handleSubmit}>검색</Button>
    </div>
  );
}

export default SearchForm;
```

#### 3) 유기체 (Organisms) 예시

분자들과 원자들을 조합하여 페이지의 특정 섹션을 구성하는 헤더를 유기체로 만들 수 있습니다.

```jsx
// src/components/organisms/Header.jsx
import React from 'react';
import SearchForm from '../molecules/SearchForm'; // 분자 컴포넌트 임포트
import Button from '../atoms/Button';           // 원자 컴포넌트 임포트

function Header() {
  const handleSearch = (term) => {
    alert(`검색: ${term}`);
  };

  return (
    <header
      style={{
        display: 'flex',
        justifyContent: 'space-between',
        alignItems: 'center',
        padding: '20px',
        backgroundColor: '#f8f9fa',
        borderBottom: '1px solid #e9ecef',
      }}
    >
      <h1 style={{ margin: 0, fontSize: '24px' }}>My App</h1>
      <SearchForm onSearch={handleSearch} />
      <nav>
        <Button onClick={() => alert('로그인')}>로그인</Button>
      </nav>
    </header>
  );
}

export default Header;
```

#### 4) 템플릿 (Templates) 예시

유기체, 분자, 원자들이 배치된 페이지의 골격 구조를 템플릿으로 나타낼 수 있습니다. 실제 데이터 대신 플레이스홀더를 사용합니다.

```jsx
// src/components/templates/HomePageTemplate.jsx
import React from 'react';
import Header from '../organisms/Header'; // 유기체 컴포넌트 임포트

function HomePageTemplate({ children }) {
  return (
    <div style={{ fontFamily: 'Arial, sans-serif' }}>
      <Header />
      <main style={{ padding: '20px' }}>
        {children} {/* 실제 페이지 콘텐츠가 들어갈 자리 */}
      </main>
      <footer
        style={{
          padding: '20px',
          backgroundColor: '#343a40',
          color: 'white',
          textAlign: 'center',
          marginTop: '40px',
        }}
      >
        <p>&copy; 2024 My App. All rights reserved.</p>
      </footer>
    </div>
  );
}

export default HomePageTemplate;
```

#### 5) 페이지 (Pages) 예시

템플릿에 실제 데이터나 콘텐츠가 채워진 최종 UI를 페이지로 만듭니다.

```jsx
// src/pages/HomePage.jsx
import React from 'react';
import HomePageTemplate from '../components/templates/HomePageTemplate'; // 템플릿 컴포넌트 임포트

function HomePage() {
  return (
    <HomePageTemplate>
      <h2>환영합니다!</h2>
      <p>이곳은 저희 웹사이트의 홈 페이지입니다. 다양한 정보를 찾아보세요.</p>
      <p>최신 소식과 업데이트를 확인하려면 아래를 참조하세요.</p>
      {/* 실제 콘텐츠 (예: 최신 게시물 목록, 추천 상품 등) */}
      <div style={{ border: '1px dashed #ccc', padding: '20px', marginTop: '20px' }}>
        <p>여기에 실제 홈 페이지 콘텐츠가 들어갑니다.</p>
        <ul>
          <li>새로운 기능 출시!</li>
          <li>이벤트 안내</li>
          <li>자주 묻는 질문</li>
        </ul>
      </div>
    </HomePageTemplate>
  );
}

export default HomePage;
```

[!NOTE]
위 예시들은 Atomic Design의 개념을 이해하기 위한 매우 간단한 코드 조각입니다. 실제 프로젝트에서는 더 복잡한 로직과 스타일링이 적용될 수 있습니다. 중요한 것은 각 단계의 컴포넌트가 어떤 역할을 하고, 어떻게 조합되는지를 이해하는 것입니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](02-Using-Route-Link-Outlet.md)
- [다음 강의로 이동](04-React-Styling.md)