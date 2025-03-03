- const davidcyrilCdn = './davidcyrilCdn.js';
if (!fs.existsSync(davidcyrilCdn)) {
    console.log('📂 File "davidcyrilCdn.js" not found. Creating...');
    
    const davidcyrilCdnContent = `
const axios = require('axios');
const FormData = require('form-data');
const fs = require('fs');
const path = require('path');

async function davidcyCdn(buffer, fileType, fileName = 'upload') {
    try {
        let tempFolder = path.join(__dirname, 'temp');
        if (!fs.existsSync(tempFolder)) fs.mkdirSync(tempFolder);

        let filePath = path.join(tempFolder, \`\${fileName}.\${fileType}\`);
        fs.writeFileSync(filePath, buffer); 

        let formData = new FormData();
        formData.append('file', fs.createReadStream(filePath));

        let response = await axios.post('http://cdn.david-cyril.net.ng/upload', formData, {
            headers: { ...formData.getHeaders() }
        });

        fs.unlinkSync(filePath); 

        if (response.data.success && response.data.files.length > 0) {
            return {
                success: true,
                url: response.data.files[0].url,
                size: response.data.files[0].size,
                name: \`\${fileName}.\${fileType}\`
            };
        } else {
            return { success: false, error: 'Upload failed' };
        }
    } catch (error) {
        return { success: false, error: error.message };
    }
}

module.exports = { davidcyCdn };
`;
    
    fs.writeFileSync(davidcyrilCdn, davidcyrilCdnContent.trim());
    console.log('✅ File "davidcyrilCdn.js" created successfully.');
}


const { davidcyCdn } = require('./davidcyrilCdn.js');



case 'url': {
    if (!m.quoted || !m.quoted.mimetype) return reply('❌ Reply to a file to upload.');

    let media = await m.quoted.download();
    let fileType = m.quoted.mimetype.split('/')[1]; 

    let result = await davidcyCdn(media, fileType);

    if (result.success) {
        reply(`✅ *Upload Successful!*\n\n📂*File Size:* ${result.size} bytes\n🔗 *URL:* ${result.url}`);
    } else {
        reply(`❌ Upload Failed: ${result.error}`);
    }
}
break;


 case 'fact':
    const bby = "https://apis.davidcyriltech.my.id/fact";

    try {
        const nyash = await axios.get(bby);
        const bwess = 'https://files.catbox.moe/ep37hx.jpg';
        const ilovedavid = nyash.data.fact;
        await David.sendMessage(m.chat, { image: { url: bwess }, caption: ilovedavid });
    } catch (error) {
        reply("An Error Occured.");
    }
    break;
    
    
    case 'imgsearch': case 'img': {
    if (!text) {
        return reply(`*Usage:* .img <query>\n\n*Example:* .img cat`);
    }

    try {
 await David.sendMessage(m.chat, { react: { text: "🔍", key: m.key } });

           const apiResponse = await axios.get(`https://apis.davidcyriltech.my.id/googleimage`, {
            params: { query: text }
        });

        const { success, results } = apiResponse.data;

        if (!success || !results || results.length === 0) {
            return reply(`❌ No images found for "${text}". Try another search.`);
        }

            const maxImages = Math.min(results.length, 5);
        for (let i = 0; i < maxImages; i++) {
            await David.sendMessage(m.chat, {
                image: { url: results[i] },
                caption: `📷 *Image Search*\n\n🔎 *Query:* "${text}"\n📄 *Result:* ${i + 1}/${maxImages}\n\n> ᴘᴏᴡᴇʀᴇᴅ ʙʏ ᴅᴀᴠɪᴅ ᴄʏʀɪʟ ᴛᴇᴄʜ*`,
            }, { quoted: m });
        }

           await David.sendMessage(m.chat, { react: { text: "✅", key: m.key } });

    } catch (error) {
        console.error("Error in Image Search:", error);
        reply(`❌ *Error fetching images. Try again later.*`);
    }
    break;
}



case 'hdimg': case 'remini': {
    if (!/image/.test(mime)) return reply(`❌ Reply to an *image* with .remini to enhance it.`);

    await David.sendMessage(m.chat, { react: { text: `⏳`, key: m.key } });

    try {
        const media = await m.quoted.download();
        const fileType = m.quoted.mimetype.split('/')[1]; 
        

        let result = await davidcyCdn(media, fileType);
        
        if (!result.success) return reply(`❌ Upload failed: ${result.error}`);

        const imageUrl = result.url;
        const enhancedImageUrl = `https://apis.davidcyriltech.my.id/remini?url=${imageUrl}`;

        await David.sendMessage(m.chat, {
            image: { url: enhancedImageUrl },
            caption: `✨ *Image Enhanced Successfully!*`
        }, { quoted: m });

        await David.sendMessage(m.chat, { react: { text: `✅`, key: m.key } });

    } catch (error) {
        console.error('Error in Remini command:', error);
        reply(`❌ Error: ${error.message}`);
    }
    break;
}


case 'removebackground': case 'removebg': {
    if (!/image/.test(mime)) return reply(`❌ Reply to an *image* with .removebg to remove background.`);

    await David.sendMessage(m.chat, { react: { text: `⏳`, key: m.key } });

    try {
        const media = await m.quoted.download();
        const fileType = m.quoted.mimetype.split('/')[1]; 
        

        let result = await davidcyCdn(media, fileType);
        
        if (!result.success) return reply(`❌ Upload failed: ${result.error}`);

        const imageUrl = result.url; 
        const removedBgUrl = `https://apis.davidcyriltech.my.id/removebg?url=${imageUrl}`;

        await Rodgers.sendMessage(m.chat, {
            image: { url: removedBgUrl },
            caption: `🖼️ *Background Removed Successfully!*`
        }, { quoted: m });

        await David.sendMessage(m.chat, { react: { text: `✅`, key: m.key } });

    } catch (error) {
        console.error('Error in RemoveBG command:', error);
        reply(`❌ Error: ${error.message}`);
    }
    break;
}

<!---
Rodgers4/Rodgers4 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
