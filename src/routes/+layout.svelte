<script>
    import '$lib/global.css'
    import { onMount } from 'svelte';
    import { auth, db } from '$lib/firebase/firebase';
    import { setDoc, getDoc, doc } from 'firebase/firestore';
    import { authStore } from '../store/store';
    import Footer from '$lib/Footer.svelte';

    const nonAuthRoutes = ['/']

    onMount(() => {
        const unsubscribe = auth.onAuthStateChanged(async user => {
            const currentPath = window.location.pathname

            if (!user && !nonAuthRoutes.includes(currentPath)) {
                window.location.href = '/'
            }

            if (user && currentPath === '/') {
                window.location.href = '/dashboard'
                return
            }

            if (!user) {
                return
            }

            // @ts-ignore
            let dataToSetToStore
            const docRef = doc(db, 'users', user.uid)
            const docSnap = await getDoc(docRef)
            if (!docSnap.exists()) {
                const userRef = doc(db, 'users', user.uid)
                dataToSetToStore = {
                    email: user.email,
                    todos: [],
                // @ts-ignore
                }
                // @ts-ignore
                await setDoc(userRef, dataToSetToStore, { merge: true })
            } else {
                const userData = docSnap.data()
                dataToSetToStore = userData
            }

            // @ts-ignore
            authStore.update(curr => {
                return {
                    ...curr,
                    user,
                    // @ts-ignore
                    data: dataToSetToStore,
                    loading: false,
                }
            })
        })
    })
</script>

<div class='main-container'>
    <slot />
    <Footer />
</div>

<style>
    .main-container {
        min-height: 100vh;
        margin: 1em;
        position: relative;
        display: flex;
        flex-direction: column;
    }
</style>