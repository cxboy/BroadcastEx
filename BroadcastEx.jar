package com.modernvillemc.broadcastex;
 
import java.util.List;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.LineNumberReader;
import java.util.Arrays;
import java.util.logging.Logger;
 
import org.bukkit.Bukkit;
import org.bukkit.ChatColor;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.plugin.PluginDescriptionFile;
import org.bukkit.plugin.java.JavaPlugin;
 
public class BroadcastEx extends JavaPlugin {
 
  //Every plugin has it's own logger you just need to get it public final Logger logger = Logger.getLogger("Minecraft");
	public Logger logger;
	public static int currentLine = 0;
	public static int tid = 0;
	public static int running = 1;
 
	@Override
	public void onDisable() {
	PluginDescriptionFile pdfFile = this.getDescription();
	//NOPE bukkit shows a msg anyways this.logger.info(pdfFile.getName() + " Version " + pdfFile.getVersion() + " has been disabled.");
	}
 
	@Override
	public void onEnable() {
		logger = getLogger();
		PluginDescriptionFile pdfFile = this.getDescription();
		//NOPE this.logger.info(pdfFile.getName() + " Version " + pdfFile.getVersion() + " has been enabled.");
 
		tid = Bukkit.getScheduler().scheduleSyncRepeatingTask(this, new Runnable() {
			public void run() {
				try {
					broadcastMessage(getConfig().getString("Messages"));
				} catch (IOException e) {
 
				}
			}
		}, 0, getConfig().getLong("interval") * 20);
 
		if (getDataFolder().exists()) {
			getLogger().info("Found config file.");
		} else {
			createConfig(new File(getDataFolder(), "config.yml"));
			getLogger().info("Config file did not exist, creating one.");
		}
	}
 
	private void createConfig(File fileName) {
		//Configurations
		// setting a boolean value
		this.getConfig().set("Auto-update", false);
 
		// setting a String
		String announcerName = "&6[&cBroadcast&5Ex&6]&f: ";
		this.getConfig().set("Announcer Name", announcerName);
 
		// setting an int value
		int Interval = 10;
		this.getConfig().set("Interval", Interval);
 
		// Setting a List of Strings
		// The List of Strings is first defined in this array
		List<String> Messages = Arrays.asList("Hello World", "Powered by BroadcastEx", "&dColor codes &fare compatible :)");
		this.getConfig().set("Messages", Messages);
		this.saveConfig();
	}
 
	public static void broadcastMessage(String fileName) throws IOException {
		FileInputStream fs;
		fs = new FileInputStream(fileName);
		BufferedReader br = new BufferedReader(new InputStreamReader(fs));
		for(int i = 0; i < currentLine; ++i);
		br.readLine();
		String line = br.readLine();
		line = ChatColor.translateAlternateColorCodes('&', line);
 
		Bukkit.getServer().broadcastMessage(ChatColor.GOLD + "[BroadcastEx]" + ChatColor.WHITE + ": " + line);
		LineNumberReader lnr = new LineNumberReader(new FileReader(new File(fileName)));
		lnr.skip(Long.MAX_VALUE);
		int lastLine = lnr.getLineNumber();
		if(currentLine + 1 == lastLine + 1) {
			currentLine = 0;
		} else {
			currentLine++;
		}
	}
 
	public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args) {
		if (commandLabel.equalsIgnoreCase("bxstop")) {
				if(running == 1) {
					Bukkit.getServer().getScheduler().cancelTask(tid);
					sender.sendMessage("BroadcastEx has stopped.");
				} else {
					sender.sendMessage("BroadcastEx is not running.");
				} } else if(commandLabel.equalsIgnoreCase("bxstart")){
					if(running == 1) {
						sender.sendMessage("BroadcastEx is already running.");
					} else {
						tid = Bukkit.getScheduler().scheduleSyncRepeatingTask(this, new Runnable() {
							public void run() {
								try {
									broadcastMessage(getConfig().getString("Messages"));
								} catch (IOException e) {							
								}
							}
						}, 0, getConfig().getLong("Interval") * 20);
	 
						if (getDataFolder().exists()) {
							getLogger().info("Found config file.");
						} else {
							createConfig(new File(getDataFolder(), "config.yml"));
							getLogger().info("Config file did not exist, creating one.");
						}
					} if (commandLabel.equalsIgnoreCase("bxinterval")){
						sender.sendMessage("The set time interval is: " + getConfig().getInt("Interval"));
					}
				}
			return false;
		}
	}
