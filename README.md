    package p.jaro.firstplugin.Commands;

    import org.bukkit.Bukkit;
    import org.bukkit.ChatColor;
    import org.bukkit.command.Command;
    import org.bukkit.command.CommandExecutor;
    import org.bukkit.command.CommandSender;
    import org.bukkit.entity.Player;
    import org.jetbrains.annotations.NotNull;
    import p.jaro.firstplugin.FirstPlugin;

    public class pvMessageCommandListener implements CommandExecutor {
        private final FirstPlugin plugin;

        public pvMessageCommandListener(FirstPlugin plugin) {
            this.plugin = plugin;
        }

        @Override
        public boolean onCommand(@NotNull CommandSender sender, @NotNull Command cmd, @NotNull String label, @NotNull String[] args) {
            if (sender instanceof Player player){
                if (cmd.getName().equalsIgnoreCase("directmessage")){
                    if (args.length==0){
                        player.sendMessage(ChatColor.RED+"Prawidlowe uzycie: "+ChatColor.DARK_AQUA+"/pv [nick] [wiadomosc]");
                    }
                    else if(args.length==1){
                        Player target = Bukkit.getServer().getPlayer(args[0]);
                        if (target!=null){
                            player.sendMessage(ChatColor.DARK_AQUA+"Podaj tresc wiadomosci");
                        }
                        else {
                            player.sendMessage(ChatColor.DARK_AQUA+args[0]+ChatColor.RED+" jest offline");
                        }
                    }
                    else if (args.length>1){
                        Player target = Bukkit.getServer().getPlayer(args[0]);
                        if (target != null) {
                            StringBuilder builder = new StringBuilder();
                            for (int i = 1; i < args.length; i++){
                                builder.append(args[i]+" ");
                            }
                            String message = builder.toString();
                            // [sendername >> target]: message
                            target.sendMessage(ChatColor.DARK_GRAY+"["+ChatColor.DARK_AQUA+player.getName()+ChatColor.GRAY+" >> "+ChatColor.AQUA+target.getName()+
                                    ChatColor.DARK_GRAY+"]: "+ChatColor.GRAY+message);
                            player.sendMessage(ChatColor.DARK_GRAY+"["+ChatColor.DARK_AQUA+player.getName()+ChatColor.GRAY+" >> "+ChatColor.AQUA+target.getName()+
                                    ChatColor.DARK_GRAY+"]: "+ChatColor.GRAY+message);
                        }
                        else {
                            player.sendMessage(ChatColor.DARK_AQUA+args[0]+ChatColor.RED+" jest offline");
                        }
                    }
                }
            }
            else {
                sender.sendMessage(ChatColor.RED+"Nie jestes graczem.");
            }

            return true;
        }
    }
