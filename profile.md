---
layout: base
title: Profile
search_exclude: true
menu: nav/mainHeader.html
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Profile | Cartage</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.css" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.js"></script>
    <style>
        .profile-pic-container {
            position: relative;
            width: 150px;
            height: 150px;
        }
        .profile-pic-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .profile-pic-container:hover .profile-pic-overlay {
            opacity: 1;
        }
        .checkmark {
            width: 20px;
            height: 20px;
            display: inline-block;
            border: 2px solid white;
            border-radius: 50%;
            position: relative;
            animation: pop .3s ease forwards;
        }
        .checkmark::after {
            content: '';
            position: absolute;
            left: 5px;
            top: 2px;
            width: 5px;
            height: 10px;
            border: solid white;
            border-width: 0 2px 2px 0;
            transform: rotate(45deg);
        }
        @keyframes pop {
            0% { transform: scale(0); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
    </style>
</head>
<body class="bg-gray-100 font-sans">
<div class="max-w-7xl mx-auto bg-white shadow-sm">

<!-- Profile Section -->
<div class="p-16 bg-white rounded-lg shadow-lg max-w-7xl mx-auto mt-10">
    <div class="flex flex-col md:flex-row space-y-10 md:space-y-0 md:space-x-16">

        <!-- Profile Picture and Role -->
        <div class="relative flex flex-col items-center">
            <img id="profile-pic"
                 src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Sample_User_Icon.png/600px-Sample_User_Icon.png"
                 alt="Profile picture"
                 class="w-40 h-40 rounded-full border-4 border-amber-500 object-cover">
        <label for="profile-pic-upload"
       class="relative group bg-amber-500 mt-4 px-5 py-2.5 rounded-md text-white text-base font-medium overflow-hidden shadow-md transition-transform duration-300 transform hover:scale-105 hover:bg-amber-600">
    <span class="absolute top-0 left-0 h-0.5 w-full bg-white transition-all duration-500 group-hover:w-0"></span>
    <span class="relative z-10">Upload Photo</span>
</label>



            <input type="file" id="profile-pic-upload" class="hidden" accept="image/*">
        </div>

        <!-- Profile Details -->
        <div class="flex-1">
            <div class="flex flex-col space-y-4 mb-8">
                <h2 id="profile-name" class="text-5xl font-bold text-gray-900">User's Name</h2>
                <p id="profile-uid" class="text-2xl text-gray-500">@userhandle</p>
                <pre id="profile-bio" class="whitespace-pre-wrap break-words break-all text-lg text-gray-600 font-medium">BIO</pre>


            </div>
            <div class="flex space-x-6 mb-6">
               <a class="bg-amber-500 hover:bg-amber-600 text-white px-6 py-3 rounded-md text-xl font-medium inline-block transition-all duration-300 shadow-md hover:scale-105">
    <i class="fas fa-envelope mr-2"></i>Message
</a>
                <button id="follow-button" class="relative bg-gray-200 text-black px-6 py-3 rounded-md text-xl font-medium flex items-center space-x-2 transition-transform duration-300 hover:scale-105">

                    <i class="fas fa-user-plus"></i>
                    <span>Follow</span>
                </button>
                <button id="edit-profile-btn" class="relative group bg-blue-600 hover:bg-blue-700 text-white px-6 py-3 rounded-md text-xl font-medium overflow-hidden transition-all duration-300 shadow-lg">
                    <span class="absolute left-0 w-full h-0.5 top-0 bg-white transition-all duration-500 group-hover:w-0"></span>
                    <i class="fas fa-user-edit mr-2"></i>Edit Profile
                </button>
            </div>
        </div>
    </div>
</div>

<!-- Cropper Modal -->
<div id="cropper-modal" class="fixed inset-0 bg-black bg-opacity-60 z-50 flex items-center justify-center hidden">
    <div class="bg-white p-4 rounded-lg shadow-lg max-w-md w-full">
        <div class="mb-4">
            <img id="cropper-image" class="max-w-full max-h-[400px]">
        </div>
        <div class="flex justify-end space-x-2">
            <button id="cancel-crop" class="bg-gray-300 hover:bg-gray-400 text-gray-700 px-4 py-2 rounded">Cancel</button>
            <button id="confirm-crop" class="bg-amber-500 hover:bg-amber-600 text-white px-4 py-2 rounded">Crop & Save</button>
        </div>
    </div>
</div>

<!-- Edit Profile Modal -->
<div id="edit-profile-modal" class="fixed inset-0 z-50 bg-black bg-opacity-50 hidden items-center justify-center">
    <div class="bg-white rounded-lg shadow-2xl p-6 w-full max-w-lg animate-scale-in">
        <h2 class="text-2xl font-bold mb-4 text-gray-800">Edit Profile</h2>
        <div class="space-y-4">
            <div>
                <label for="name-input" class="block text-gray-700 font-semibold mb-1">Name</label>
                <input id="name-input" type="text" class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-400">
            </div>
            <div>
                <label for="bio-input" class="block text-gray-700 font-semibold mb-1">BIO</label>
               <div class="relative">
    <textarea id="bio-input" rows="3" class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-400"></textarea>
    <p id="bio-word-count" class="absolute bottom-1 right-2 text-xs text-gray-500">0 words</p>
</div>

            </div>
        </div>
        <div class="mt-6 flex justify-end space-x-3">
            <button id="cancel-edit" class="px-4 py-2 rounded-md bg-gray-200 hover:bg-gray-300 text-gray-700">Cancel</button>
            <button id="save-edit" class="px-4 py-2 rounded-md bg-blue-600 hover:bg-blue-700 text-white">Save</button>
        </div>
    </div>
</div>

<!-- Animations -->
<style>
    @keyframes scale-in {
        from { transform: scale(0.9); opacity: 0; }
        to { transform: scale(1); opacity: 1; }
    }

    .animate-scale-in {
        animation: scale-in 0.3s ease-out;
    }
</style>

<!-- Scripts -->
<script type="module">
    import { pythonURI, fetchOptions } from "{{site.baseurl}}/assets/js/api/config.js"
    let cropper;
    const profilePic = document.getElementById('profile-pic');
    const uploadInput = document.getElementById('profile-pic-upload');
    const cropperModal = document.getElementById('cropper-modal');
    const cropperImage = document.getElementById('cropper-image');
    const cancelCropBtn = document.getElementById('cancel-crop');
    const confirmCropBtn = document.getElementById('confirm-crop');
    const followButton = document.getElementById('follow-button');
    let following = false;

    async function getUserData() {
        const endpoint = `${pythonURI}/api/user`
        try {
            const response = await fetch(endpoint, fetchOptions);
            if (!response.ok) {
                throw new Error(`Failed to fetch user: ${response.status}`);
            }
            const user = await response.json();
            return user;
        } catch (error) {
            console.error("Error fetching user:", error.message);
            return null;
        }
    }

    async function getPfp() {
      return fetch(`${pythonURI}/api/pfp`, fetchOptions)
        .then(response => response.json())
        .then(result => {
          console.log()
        return result.pfp
        })
        .catch(err => {
        console.error("ERROR: ", err)
        return
      })
    }

    async function updateUser(data) {
        const endpoint = `${pythonURI}/api/user`;
        try {
            const response = await fetch(endpoint, {
                ...fetchOptions,
                method: "PUT",
                headers: {
                    ...fetchOptions.headers,
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(data)
            });

            if (!response.ok) {
                throw new Error(`Failed to update user: ${response.status}`);
            }

            return await response.json();
        } catch (error) {
            console.error("Error updating user:", error.message);
        }
    }


    document.addEventListener("DOMContentLoaded", async () => {
        const userData = await getUserData()
        const pfp = await getPfp()

        if (!userData) {
            console.error("no user data")
        }

        document.getElementById("profile-name").textContent = userData.name;
        document.getElementById("profile-uid").textContent = `@${userData.uid}`;
        document.getElementById("profile-bio").textContent = userData.bio;
        document.getElementById("name-input").value = userData.name;
        document.getElementById("bio-input").textContent = userData.bio;
        document.getElementById("profile-pic").src = pfp
        //UPDATE PFP TOO
    })

    uploadInput.addEventListener('change', function () {
        const file = this.files[0];
        if (file) {
            const reader = new FileReader();
            reader.onload = function (e) {
                cropperImage.src = e.target.result;
                cropperModal.classList.remove('hidden');
                if (cropper) cropper.destroy();
                cropper = new Cropper(cropperImage, {
                    aspectRatio: 1,
                    viewMode: 1,
                    dragMode: 'move',
                    guides: false,
                    background: false,
                    autoCropArea: 1,
                    movable: true,
                    zoomable: true
                });
            };
            reader.readAsDataURL(file);
        }
    });

    cancelCropBtn.addEventListener('click', () => {
        cropperModal.classList.add('hidden');
        uploadInput.value = '';
        if (cropper) cropper.destroy();
    });

    confirmCropBtn.addEventListener('click', () => {
        const canvas = cropper.getCroppedCanvas({
            width: 300,
            height: 300,
            imageSmoothingQuality: 'high'
        });
        const mimeType = uploadInput.files[0].type
        profilePic.src = canvas.toDataURL(mimeType);

        canvas.toBlob((blob) => {
          if (!blob) {
            console.error('Canvas is empty');
            return;
          }

          // Create a FormData object
          const formData = new FormData();
          formData.append('file', blob, 'profile.png'); // **Uploads the image (sends the image to server)**

          // Send the form data via fetch
          fetch(pythonURI+"/api/upload", {
            method: 'POST',
            mode: "cors", // no-cors, *cors, same-origin
            cache: "default", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "include", // include, same-origin, omit
            headers: {
              "X-Origin": "client", // New custom header to identify source
            },
            body: formData
          })
          .then(response => response.json())
          .then(result => { // **Gets back a unique ID for the image**
            fetch(pythonURI+"/api/pfp", {
              method: 'PUT',
              mode: "cors", // no-cors, *cors, same-origin
              cache: "default", // *default, no-cache, reload, force-cache, only-if-cached
              credentials: "include", // include, same-origin, omit
              headers: {
                "Content-Type": "application/json",
                "X-Origin": "client", // New custom header to identify source
              },
              body: JSON.stringify({
                image_uuid: result["UUID"]
              })
            })
            .then(response => response.json())
            .then(result => {
              console.log(result)
              fetch(pythonURI+"/api/pfp", { // **Tells the server to set that image as the profile picture**
                method: 'GET',
                mode: "cors", // no-cors, *cors, same-origin
                cache: "default", // *default, no-cache, reload, force-cache, only-if-cached
                credentials: "include", // include, same-origin, omit
                headers: {
                  "Content-Type": "application/json",
                  "X-Origin": "client", // New custom header to identify source
                }
              }).then(response => response.json()).then(result => {
                console.log(result)
              })

            })
            .catch(error => {
              console.error('Error:', error);
            })
          })
          .catch(error => {
            console.error('Error:', error);
          });
        }, mimeType); // MIME type

        cropperModal.classList.add('hidden');
        uploadInput.value = '';
        if (cropper) cropper.destroy();
    });

    followButton.addEventListener('click', function () {
        following = !following;
        if (following) {
            this.innerHTML = '<div class="checkmark"></div><span class="ml-2">Following</span>';
            this.classList.remove('bg-gray-200', 'text-black');
            this.classList.add('bg-green-500', 'text-white');
        } else {
            this.innerHTML = '<i class="fas fa-user-plus"></i><span class="ml-2">Follow</span>';
            this.classList.remove('bg-green-500', 'text-white');
            this.classList.add('bg-gray-200', 'text-black');
        }
    });

    // Edit Profile
    const editBtn = document.getElementById('edit-profile-btn');
    const modal = document.getElementById('edit-profile-modal');
    const cancelEdit = document.getElementById('cancel-edit');
    const saveEdit = document.getElementById('save-edit');
    const nameInput = document.getElementById('name-input');
    const bioInput = document.getElementById('bio-input');
    const profileName = document.getElementById('profile-name');
    const profileBio = document.getElementById('profile-bio');

const MAX_CHARS = 300;
const bioWordCount = document.getElementById('bio-word-count');

bioInput.addEventListener('input', () => {
    // Trim excess characters if over limit
    if (bioInput.value.length > MAX_CHARS) {
        bioInput.value = bioInput.value.slice(0, MAX_CHARS);
    }

    const charCount = bioInput.value.length;
    bioWordCount.textContent = `${charCount}/${MAX_CHARS} characters`;

    // Toggle color
    if (charCount >= MAX_CHARS) {
        bioWordCount.classList.add('text-red-500');
        bioWordCount.classList.remove('text-gray-500');
    } else {
        bioWordCount.classList.remove('text-red-500');
        bioWordCount.classList.add('text-gray-500');
    }
});


  editBtn.addEventListener('click', () => {
    modal.classList.remove('hidden');
    modal.classList.add('flex');

    // Update the word count when opening the modal
    const charCount = bioInput.value.length;
bioWordCount.textContent = `${charCount}/${MAX_CHARS} characters`;

});


    cancelEdit.addEventListener('click', () => {
        modal.classList.add('hidden');
        modal.classList.remove('flex');
    });

    saveEdit.addEventListener('click', async () => {
    const updatedName = nameInput.value;
    const updatedBio = bioInput.value;

    // Update UI immediately
    profileName.textContent = updatedName;
    profileBio.textContent = updatedBio;

    // Close modal
    modal.classList.add('hidden');
    modal.classList.remove('flex');

    // Send to backend
    await updateUser({
        name: updatedName,
        bio: updatedBio
    });
});


document.addEventListener('keydown', (e) => {
    const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
    const isEditShortcut = (isMac && e.metaKey && e.key.toLowerCase() === 'e');

    if (isEditShortcut) {
        e.preventDefault(); // prevent default browser behavior
        modal.classList.remove('hidden');
        modal.classList.add('flex');

        const charCount = bioInput.value.length;
        bioWordCount.textContent = `${charCount}/${MAX_CHARS} characters`;
    }
});

document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape' && !modal.classList.contains('hidden')) {
        e.preventDefault(); // Stop system/minimize behavior
        modal.classList.add('hidden');
        modal.classList.remove('flex');
    }
});



</script>
