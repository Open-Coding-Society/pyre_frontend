---
layout: post
permalink: /profile
#menu: nav/home.html
search_exclude: true
show_reading_time: false
---

<style>
    /* ...unchanged CSS... */
    body {
        background: #18181b;
        color:rgb(226, 132, 132);
    }
    .card, .profile, .posts-container, .message-bubble {
        background: #232336;
        color: #f3f4f6;
    }
    .input-field, input[type="text"], input[type="password"], textarea {
        background: #18181b;
        color: #f3f4f6;
        border: 1px solid #444;
    }
    label {
        color: #e0e0e0;
    }
    .btn-primary {
        background:rgb(0, 0, 0);
        color: #fff;
    }
    .btn-primary:hover {
        background:rgb(0, 0, 0);
    }
    .file-icon {
        color: #a5b4fc;
    }
    .post-title {
        color: #a5b4fc;
        font-weight: bold;
    }
    .post-comment {
        color: #f3f4f6;
    }
    #profile-message {
        color: #f87171;
    }
    a {
        color: #60a5fa;
    }
</style>

<header class="heading">
    <h1>Pyre</h1>
    <p>You can control your settings from here!</p>
</header>

<div class="theme-switch-wrapper">
    <!-- Theme toggle button removed -->
</div>

<div class="container">
    <section class="profile">
        <img src="https://placehold.co/150x150" alt="Profile Picture" id="profilePicture">
        <div>
            <h2 id="username">User Name</h2>
        </div>
    </section>

    <section class="card">
        <h3>Profile Settings</h3>
        <form>
            <div>
                <label for="newUid">Enter New UID:</label>
                <input type="text" id="newUid" placeholder="New UID">
            </div>
            <div>
                <label for="newName">Enter New Name:</label>
                <input type="text" id="newName" placeholder="New Name">
            </div>
            <div>
                <label for="newPassword">Enter New Password:</label>
                <input type="password" id="newPassword" placeholder="New Password">
            </div>
            <br>
            <label for="profilePictureUpload" class="file-icon">
                Upload Profile Picture <i class="fas fa-upload"></i>
            </label>
            <input type="file" id="profilePictureUpload" accept="image/*" style="display: none;">
            <p id="profile-message"></p>
        </form>
    </section>

    <section class="grid grid-cols-2">
        <div class="card">
            <h3>User Stats</h3>
            <p>Followers: 120</p>
            <p>Following: 75</p>
            <p>Posts: 34</p>
        </div>
        <div class="card">
            <h3>Bio/About Me</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur nec felis vel metus.</p>
        </div>
    </section>

    <section class="card">
        <h3>Create New Post</h3>
        <form id="newPostForm">
            <div>
                <label for="postTitle">Title</label>
                <input type="text" id="postTitle" name="postTitle" class="input-field" placeholder="Enter post title">
            </div>
            <div>
                <label for="postComment">Comment</label>
                <textarea id="postComment" name="postComment" class="input-field" placeholder="Enter your comment" rows="4"></textarea>
            </div>
            <button type="button" onclick="createPost()" class="btn btn-primary">Create Post</button>
        </form>
    </section>

    <section class="card">
        <h3>Recent Posts</h3>
        <div id="recentPosts" class="posts-container">
        </div>
    </section>

    <section class="card">
        <h3>Activity Feed</h3>
        <ul>
            <li>User1 liked your post</li>
            <li>User2 commented on your photo</li>
            <li>User3 started following you</li>
        </ul>
    </section>
    <br>
</div>

<script type="module">
import { pythonURI, fetchOptions } from '{{site.baseurl}}/assets/js/api/config.js';


async function updateUserInfo() {
    try {
        const response = await fetch(pythonURI + "/api/user", {
            ...fetchOptions,
            method: 'GET'
        });
        const data = await response.json();
        
        document.getElementById('username').textContent = data.name || 'User Name';
        
        if (data.pfp) {
            document.getElementById('profilePicture').src = data.pfp;
        }
    } catch (error) {
        console.error('Error fetching user info:', error);
    }
}

document.addEventListener('DOMContentLoaded', updateUserInfo);

async function fetchProfilePicture() {
    try {
        const response = await fetch(pythonURI + "/api/id/pfp", fetchOptions);
        if (!response.ok) {
            throw new Error('Failed to fetch profile picture');
        }
        const data = await response.json();
        if (data && data.pfp) {
            document.getElementById('profilePicture').src = `data:image/jpeg;base64,${data.pfp}`;
        }
    } catch (error) {
        console.error('Error fetching profile picture:', error);
        showError('Error fetching profile picture');
    }
}

function setPlaceholders(userData) {
    const uidInput = document.getElementById('newUid');
    const nameInput = document.getElementById('newName');

    if (userData.uid) uidInput.placeholder = userData.uid;
    if (userData.name) nameInput.placeholder = userData.name;
}

async function updateProfile(field, value) {
    try {
        const response = await fetch(pythonURI + "/api/user", {
            ...fetchOptions,
            method: 'PUT',
            body: JSON.stringify({
                [field]: value
            })
        });

        if (!response.ok) {
            throw new Error('Failed to update profile');
        }

        showError('Profile updated successfully', 'green');
        updateUserInfo();
    } catch (error) {
        console.error('Error updating profile:', error);
        showError(error.message || 'Error updating profile');
    }
}

function showError(message, color = 'red') {
    const messageElement = document.getElementById('profile-message');
    messageElement.style.color = color;
    messageElement.textContent = message;
    setTimeout(() => {
        messageElement.textContent = '';
    }, 3000);
}

document.addEventListener('DOMContentLoaded', () => {
    fetchProfilePicture();

    const profilePictureInput = document.getElementById('profilePictureUpload');
    profilePictureInput.addEventListener('change', (e) => {
        if (e.target.files[0]) {
            uploadProfilePicture(e.target.files[0]);
        }
    });

    const inputs = ['newUid', 'newName', 'newPassword'];
    inputs.forEach(id => {
        const input = document.getElementById(id);
        input.addEventListener('change', (e) => {
            if (e.target.value) {
                updateProfile(id.replace('new', '').toLowerCase(), e.target.value);
                e.target.value = '';
            }
        });
    });

    // Theme toggle logic removed
});

window.onload = function() {
    fetchPosts();
};

async function fetchPosts() {
    const channelData = {
        channel_id: 7 
    };

    try {
        const response = await fetch(`${pythonURI}/api/posts/filter`, {
            ...fetchOptions,
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(channelData)
        });

        if (!response.ok) {
            throw new Error('Failed to get posts: ' + response.statusText);
        }

        const posts = await response.json();
        const postsContainer = document.getElementById('recentPosts');
        postsContainer.innerHTML = '';

        posts.forEach(post => {
            const postElement = document.createElement('div');
            postElement.classList.add('message-bubble');
            
            const titleElement = document.createElement('div');
            titleElement.classList.add('post-title');
            titleElement.textContent = post.title;
            
            const commentElement = document.createElement('div');
            commentElement.classList.add('post-comment');
            commentElement.textContent = post.comment;
            
            postElement.appendChild(titleElement);
            postElement.appendChild(commentElement);
            postsContainer.appendChild(postElement);
        });

    } catch (error) {
        console.error('Error fetching posts:', error);
        const postsContainer = document.getElementById('recentPosts');
        postsContainer.innerHTML = '<p style="color: #e53e3e;">Error loading posts. Please try again later.</p>';
    }
}

async function createPost() {
    const title = document.getElementById('postTitle').value.trim();
    const comment = document.getElementById('postComment').value.trim();
    
    if (!title || !comment) {
        alert('Please fill in both title and comment fields');
        return;
    }

    const postData = {
        title: title,
        comment: comment,
        channel_id: 7,
    };

    try {
        const response = await fetch(`${pythonURI}/api/post`, {
            ...fetchOptions,
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(postData)
        });

        if (!response.ok) {
            throw new Error('Failed to create post: ' + response.statusText);
        }

        document.getElementById('newPostForm').reset();
        alert('Post created successfully!');
        
        // Refresh posts after creating a new one
        await fetchPosts();
        
    } catch (error) {
        console.error('Error creating post:', error);
        alert('Failed to create post. Please try again.');
    }
}

window.createPost = createPost;
</script>
