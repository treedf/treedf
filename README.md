mkdir social-media-app
cd social-media-app
npm init -y
npm install express mongoose body-parser bcrypt jsonwebtoken
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function App() {
    const [username, setUsername] = useState('');
    const [password, setPassword] = useState('');
    const [posts, setPosts] = useState([]);
    const [content, setContent] = useState('');

    const signup = async () => {
        await axios.post('http://localhost:5000/signup', { username, password });
        alert('User created!');
    };

    const login = async () => {
        const res = await axios.post('http://localhost:5000/login', { username, password });
        localStorage.setItem('token', res.data.token);
        alert('Login successful!');
    };

    const fetchPosts = async () => {
        const res = await axios.get('http://localhost:5000/posts');
        setPosts(res.data);
    };

    const createPost = async () => {
        const token = localStorage.getItem('token');
        await axios.post('http://localhost:5000/posts', { content, author: username }, {
            headers: { Authorization: Bearer ${token} }
        });
        fetchPosts();
    };

    useEffect(() => {
        fetchPosts();
    }, []);

    return (
        <div>
            <h1>Social Media App</h1>
            <div>
                <h2>Signup</h2>
                <input placeholder="Username" onChange={(e) => setUsername(e.target.value)} />
                <input type="password" placeholder="Password" onChange={(e) => setPassword(e.target.value)} />
                <button onClick={signup}>Signup</button>
            </div>
            <div>
                <h2>Login</h2>
                <input placeholder="Username" onChange={(e) => setUsername(e.target.value)} />
                <input type="password" placeholder="Password" onChange={(e) => setPassword(e.target.value)} />
                <button onClick={login}>Login</button>
            </div>
            <div>
                <h2>Create Post</h2>
                <textarea placeholder="Content" onChange={(e) => setContent(e.target.value)}></textarea>
                <button onClick={createPost}>Post</button>
            </div>
            <div>
                <h2>Posts</h2>
                {posts.map((post, index) => (
                    <div key={index}>
                        <p><b>{post.author}</b>: {post.content}</p>
                    </div>
                ))}
            </div>
        </div>
    );
}

