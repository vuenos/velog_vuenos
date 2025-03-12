# Zustand + AccessToken + cookies-next 로 인증로직 구현

## Zustand Store 생성
- [공식문서](https://zustand-demo.pmnd.rs/)
- 로그인 로직에서 ```setAuthState``` 액션을 이용해서 'isAuthenticated' 상태를 변화시켜 인증상태를 유지시킴

```javascript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

// 인증 상태 인터페이스
interface AuthState {
    isAuthenticated: boolean;
    setAuthState: (
        isAuthenticated: boolean,
        userRole: string | null,
        userName: string | null,
    ) => void;
    resetAuthState: () => void;
}

export const useAuthStore = create<AuthState>()(
    persist(
        (set) => ({
            isAuthenticated: false,
            userRole: null,
            userEmail: null,
            setAuthState: (isAuthenticated, userRole, userEmail) =>
                set({ isAuthenticated, userRole, userName }),
            resetAuthState: () =>
                set({
                    isAuthenticated: false,
                    userRole: null,
                    userName: null,
                }),
        }),
        {
            name: 'auth-storage', // localStorage 키 이름
            partialize: (state) => ({
                userName: state.userName,
                userRole: state.userRole,
            }), // 로컬 스토리지 사용
        },
    ),
);

```