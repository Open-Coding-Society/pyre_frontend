---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /signin/
---

<div class="min-h-screen w-full flex flex-col items-center justify-between bg-black bg-opacity-95 bg-[url('https://images.unsplash.com/photo-1518791841217-8f162f1e1131?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80')] bg-cover bg-center bg-blend-overlay">

  <br>
  <br>
  <br>
  <br>

  <!-- Login form container -->
  <div class="flex flex-col items-center justify-center w-full max-w-md px-4">
    <div class="w-full bg-gray-900 bg-opacity-80 rounded-lg shadow-xl p-8 backdrop-blur">
      <h2 class="text-3xl font-bold text-white text-center mb-2">Sign in to your account</h2>
      <p class="text-gray-400 text-center mb-6">Monitor hotspots and stay safe</p>
      <form class="space-y-4">
        <div>
          <input type="email" placeholder="Email address" class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div>
          <input type="password" placeholder="Password" class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div class="flex items-center justify-between">
          <div class="flex items-center">
            <input type="checkbox" id="remember" class="w-4 h-4 bg-gray-800 border-gray-700 rounded focus:ring-orange-500 focus:ring-opacity-25">
            <label for="remember" class="ml-2 text-sm text-gray-400">Remember me</label>
          </div>
          <a href="#" class="text-sm text-orange-500 hover:text-orange-400">Forgot your password?</a>
        </div>
        <button type="submit" class="w-full py-3 px-4 bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white font-medium rounded shadow-lg transition duration-300 flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 16l-4-4m0 0l4-4m-4 4h14"/>
          </svg>
          Sign in
        </button>
      </form>
      <div class="text-center mt-6">
        <p class="text-gray-400 text-sm">
          Don't have an account?
          <a href="#" class="text-orange-500 hover:text-orange-400 font-medium">Create one now</a>
        </p>
      </div>
    </div>
  </div>
  <footer class="w-full py-6 text-center">
    <div class="text-gray-500 text-sm mb-2">© 2025 Pyre. All rights reserved.</div>
    <div class="flex justify-center space-x-4">
      <a href="#" class="text-gray-500 hover:text-gray-400 text-sm">Terms of Service</a>
      <a href="#" class="text-gray-500 hover:text-gray-400 text-sm">Privacy Policy</a>
    </div>
  </footer>
</div>