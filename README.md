# chrome_setup

import subprocess
import platform
import os
import time

def setup_chrome_debugging():
    """Setup Chrome with remote debugging and save the debug address"""
    system = platform.system()
    
    # Determine Chrome executable path and debug file location based on OS
    if system == 'Windows':
        chrome_paths = [
            r'C:\Program Files\Google\Chrome\Application\chrome.exe',
            r'C:\Program Files (x86)\Google\Chrome\Application\chrome.exe',
            os.path.expanduser('~') + r'\AppData\Local\Google\Chrome\Application\chrome.exe'
        ]
        debug_file = os.path.expanduser('~') + '\\chrome_debug.txt'
        
        # Find the first existing Chrome path
        chrome_path = next((path for path in chrome_paths if os.path.exists(path)), None)
        if not chrome_path:
            raise Exception("Chrome not found in standard locations")
            
    else:  # Linux/MacOS
        if system == 'Darwin':  # MacOS
            chrome_path = '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome'
        else:  # Linux
            chrome_path = 'google-chrome'
        debug_file = os.path.expanduser('~') + '/chrome_debug.txt'

    # Create a unique debugging port
    debug_port = 9222
    debug_address = f'127.0.0.1:{debug_port}'
    
    # Launch Chrome with debugging enabled
    try:
        if system == 'Windows':
            cmd = [
                chrome_path,
                f'--remote-debugging-port={debug_port}',
                '--user-data-dir=remote-profile',
                '--no-first-run',
                '--no-default-browser-check'
            ]
        else:
            cmd = [
                chrome_path,
                f'--remote-debugging-port={debug_port}',
                '--user-data-dir=remote-profile',
                '--no-first-run',
                '--no-default-browser-check'
            ]
        
        # Start Chrome process
        subprocess.Popen(cmd)
        
        # Save debugging address to file
        with open(debug_file, 'w') as f:
            f.write(debug_address)
            
        print(f"""
Chrome has been started in debugging mode!

1. A new Chrome window should have opened
2. Log in to VFS Global in this window
3. Keep this Chrome window open
4. Run the VFS monitor script in a separate terminal

Debug address has been saved to: {debug_file}
        """)
        
    except Exception as e:
        print(f"Error starting Chrome: {str(e)}")
        raise

if __name__ == "__main__":
    setup_chrome_debugging()