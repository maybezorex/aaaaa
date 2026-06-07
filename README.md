const tiktok = require('@robinpath/tiktok');

async function removeAllLikedVideos(sessionId) {
    // Set your session credentials
    tiktok.setCredentials({ sessionId: sessionId });
    
    console.log('Fetching your liked videos...');
    
    // Get list of your liked videos
    const likedVideos = await tiktok.listVideos({
        type: 'liked',
        count: 100  // Adjust as needed
    });
    
    console.log(`Found ${likedVideos.length} liked videos`);
    
    // Unlike each one
    for (let i = 0; i < likedVideos.length; i++) {
        const video = likedVideos[i];
        try {
            await tiktok.unlikeVideo(video.id);
            console.log(`[${i+1}/${likedVideos.length}] Unliked: ${video.id}`);
        } catch (err) {
            console.log(`[${i+1}/${likedVideos.length}] Failed: ${video.id}`);
        }
        
        // Rate limiting - don't spam TikTok
        await new Promise(resolve => setTimeout(resolve, 1000));
    }
    
    console.log('Done!');
}

// Run it - replace with your actual session ID
const YOUR_SESSION_ID = 'ae9806a939f734832dc7143275f7eb60';
removeAllLikedVideos(YOUR_SESSION_ID);