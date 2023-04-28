import org.bukkit.Material;
import org.bukkit.enchantments.Enchantment;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerMoveEvent;
import org.bukkit.inventory.ItemStack;
import java.util.Random;

import java.util.Arrays;
import java.util.List;
import java.util.Random;

public class SeuPlugin implements Listener {
    private static final List<Enchantment> POSSIBLE_ENCHANTMENTS = Arrays.asList(
            Enchantment.SHARPNESS,
            Enchantment.PROTECTION_ENVIRONMENTAL,
            Enchantment.FIRE_ASPECT
    );

    private final Random random = new Random();

    @EventHandler
    public void onPlayerMove(PlayerMoveEvent event) {
        Player player = event.getPlayer();
        int distance = (int) player.getLocation().distance(event.getFrom());

        if (distance >= 10) {
            ItemStack[] items = player.getInventory().getContents();
            for (ItemStack item : items) {
                if (item != null && item.getType() != Material.AIR) {
                    Enchantment enchantment = getRandomEnchantment();
                    int currentLevel = item.getEnchantmentLevel(enchantment);
                    if (currentLevel < 1000) {
                        item.addUnsafeEnchantment(enchantment, currentLevel + 1);
                    }
                }
            }
            player.getInventory().setContents(items);
        }
    }

    private Enchantment getRandomEnchantment() {
        int index = random.nextInt(POSSIBLE_ENCHANTMENTS.size());
        return POSSIBLE_ENCHANTMENTS.get(index);
